<script setup>
const path = ref("terminal@kamilkruszona.dev:~$ ");
const declaredCommands = {
  help: [
    "about - coś o stronie",
    "secret - ???? (potrzebne hasło, jest gdzieś na stronie może F5)",
    "clear/cls - czyszczenie",
    "creator - Kamil Kruszona",
  ],
  about:
    "Strona została napisana z użyciem frameworka nuxt i tailwindcss. Jest hostowana na vercel.com z podłączoną domeną od Google Domains.",
  creator: "Pozdrawiam.",
};
const commands = ref([]);
const value = ref("");
const used = ref(false);
const used2 = ref(false);
const startingCoolOS = ref(true);
const startingCoolOSCommands = ref(["Ładowanie sapiruntuOS..."]);
console.log("Ehhh, udało się znaleźć hasło: wot");
function onEnter() {
  commands.value.push(value.value);
  value.value = "";
}
function clearing() {
  commands.value = [];
}
function start() {
  if (!used2.value) {
    const alerts = [
      "Łączenie z bazą smutnyDB",
      "Uruchamianie quizów",
      "Nie udało się uruchomić wifi",
      "Błąd xfcee4, nie da się żyć",
      "Adam left the server",
      "A może jednak F12",
      "Uruchamianie terminalu",
      false,
    ];
    const stdTimeOut = 500;
    let timeOut = 500;
    alerts.forEach((e, i) => {
      timeOut = stdTimeOut * (i + 1);
      if (e) {
        setTimeout(() => {
          startingCoolOSCommands.value.push(e);
        }, timeOut);
      } else {
        setTimeout(() => {
          startingCoolOS.value = false;
        }, timeOut);
      }
    });
  }
  used2.value = true;
}
function loading() {
  if (!used.value) {
    setTimeout(() => {
      window.open(
        "https://www.youtube.com/watch?v=dQw4w9WgXcQ&ab_channel=RickAstley",
        "_blank"
      );
    }, 1000);
  }
  used.value = true;
}
onMounted(() => {
  window.addEventListener("click", () => {
    document.querySelector("input").focus();
  });
});
useHead({
  htmlAttrs: {
    lang: "pl",
  },
  bodyAttrs: {
    class: "bg-black w-full text-white",
  },
  title: "Terminal",
  meta: [
    {
      hid: "description",
      name: "description",
      content: "Strona typu zaskocz mnie",
    },
  ],
  link: [
    {
      rel: "icon",
      type: "image/x-icon",
      href: "/favicon.ico",
    },
  ],
});
</script>
<template>
  <div v-if="startingCoolOS" class="m-2 flex flex-col">
    <span
      v-for="loadingCommands in startingCoolOSCommands"
      :key="loadingCommands"
      class="flex flex-col"
    >
      <p>{{ loadingCommands }}</p>
    </span>
    {{ start() }}
  </div>
  <div v-else class="m-2">
    <div>
      <div v-for="command in commands" :key="command">
        <p class="block whitespace-nowrap text-green-600">
          {{ path }} {{ command }}
        </p>
        <p v-if="command == 'help'" class="ml-6 flex flex-col">
          <span class="w-1/2" v-for="el in declaredCommands.help" :key="el">
            {{ el }}
          </span>
        </p>
        <p v-else-if="declaredCommands[command]" class="ml-6 w-1/2">
          {{ declaredCommands[command] }}
        </p>
        <p v-else-if="command === 'secret'" class="ml-6 w-1/2">
          Poprawne użycie to secret -pass HASŁO
        </p>
        <p v-else-if="command === 'secret -pass wot'" class="ml-6 w-1/2">
          Ładowanie.... {{ loading() }}
        </p>
        <p v-else-if="command.match(/secret -pass/g)" class="ml-6 w-1/2">
          Złe hasło
        </p>
        <p
          v-else-if="command === 'clear' || command === 'cls'"
          class="ml-6 w-1/2"
        >
          {{ clearing() }}
        </p>
        <p v-else class="ml-6">
          Nie mogę znaleźć komendy. Użyj
          <span class="text-blue-600">help</span> aby sprawdzić wszystkie
          dostępne
        </p>
      </div>
    </div>
    <div>
      <p class="text-green-500">
        {{ path }}
        {{ value }}
        <span class="bg-green-300 animate-pulse">&nbsp;&nbsp;</span>
      </p>
      <input v-model="value" class="h-0" type="text" @keydown.enter="onEnter" />
    </div>
  </div>
</template>
