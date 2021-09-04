<template>
  <div id="app" class="min-h-screen bg-gray-50">
    <div class="flex flex-row min-h-screen max-h-screen overflow-hidden">
      <aside class="flex flex-col w-1/2 sm:w-1/4 border-r bg-indigo-200">
        <header>
          <img src="./assets/header.jpg" class="w-full h-auto" />
        </header>
        <div class="flex flex-row bg-indigo-600">
          <input
            type="text"
            placeholder="Search"
            class="px-2 py-2 bg-transparent text-white placeholder-white outline-none flex-grow"
            v-model="filterActions"
          />
          <button
            class="text-white px-2"
            @click="filterActions = ''"
            v-if="filterActions.length > 0"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-6 w-6"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
          </button>
        </div>
        <nav
          class="flex-1 min-h-0 overflow-y-auto"
          aria-label="List of ghost actions"
        >
          <ul>
            <li
              class="p-2 border-b border-indigo-300 cursor-pointer"
              :class="{
                'bg-gray-200': isSelected(a),
                'border-t border-indigo-300': i === 0,
              }"
              v-for="(a, i) in sortedActions"
              :key="a"
              @click="selectAction(a)"
            >
              <div class="flex flex-row justify-between items-center">
                <h3 class="sm:text-xl md:text-2xl">{{ a }}</h3>
              </div>
            </li>

            <li v-if="sortedActions.length === 0">
              <div class="flex flex-row justify-between items-center">
                <h3 class="sm:text-lg md:text-xl w-full text-center py-4">
                  No actions found.
                </h3>
              </div>
            </li>
          </ul>
        </nav>
      </aside>
      <main class="flex flex-col w-1/2 sm:w-3/4 relative">
        <div
          class="flex flex-col sm:flex-row w-full absolute justify-between p-4"
        >
          <div class="flex flex-col">
            <button
              class="block text-white h-10 px-4 uppercase font-semibold mr-4 text-xs md:text-base"
              :class="{
                'bg-gray-700 cursor-not-allowed': selected.length === 0,
                'bg-green-700': selected.length > 0,
              }"
              :disabled="selected.length === 0"
              @click="selected = []"
            >
              Reset Selection
            </button>

            <button
              class="block text-white h-10 px-4 uppercase font-semibold mr-4 text-xs md:text-base sm:mb-0 mt-4"
              :class="{ 'bg-blue-700': !checklist, 'bg-red-700': checklist }"
              @click="checklist = !checklist"
            >
              {{ checklist ? `Ghost Mode` : `Checklist Mode` }}
            </button>
          </div>

          <div class="hidden sm:flex flex-col sm:flex-row">
            <a
              class="flex items-center text-center text-white px-4 h-10 uppercase font-semibold steam mr-4 text-xs md:text-base"
              href="https://store.steampowered.com/app/1708460/Obsideo/"
              title="Buy Obsideo now on the Official Steam Store."
            >
              Steam Store
            </a>

            <a
              class="flex items-center text-center text-white px-4 h-10 uppercase font-semibold discord mr-4 sm:mr-4 md:mr-0 text-xs md:text-base"
              href="https://discord.gg/obsideo"
              title="Join Obsideo's awesome community on discord."
            >
              Join Discord
            </a>
          </div>
        </div>
        <div class="flex flex-col flex-grow items-center justify-center">
          <h2
            class="sm:text-xl md:text-2xl text-gray-600 mb-4"
            v-if="
              selected.length > 0 && this.possibility.length > 0 && !checklist
            "
          >
            Your ghost could be...
          </h2>
          <h2
            class="sm:text-xl md:text-2xl text-gray-600 mb-4 px-8 text-center"
            v-if="selected.length === 0"
          >
            Select an evidence first and your ghost types will appear here.

            <br />
            <br />

            If you just wish to use this to keep tabs on what evidence you have,
            select checklist mode.
          </h2>

          <h2
            class="sm:text-xl md:text-2xl text-gray-600 mb-4 px-8 text-center"
            v-if="
              selected.length > 0 && this.possibility.length === 0 && !checklist
            "
          >
            We could not find a match.
            <span>
              This could be you've entered something wrong, due to a bug with
              our website or due to a bug with the game.
            </span>
          </h2>
          <div class="text-center" v-if="!checklist">
            <h1
              class="text-xl sm:text-2xl md:text-4xl"
              v-for="p in possibility"
              :key="p"
            >
              {{ ucFirst(p) }}
            </h1>

            <p class="mt-4" v-if="possibility.length === 1">
              <span class="text-lg font-bold text-red-700">
                {{ getItems(possibility[0]) }}
              </span>
            </p>
          </div>

          <div class="text-center" v-else>
            <h1
              class="text-xl sm:text-2xl md:text-4xl mb-1"
              v-for="s in selected"
              :key="s"
            >
              {{ s }}
            </h1>
          </div>
        </div>

        <footer
          class="flex flex-col w-full h-12 md:h-20 absolute bottom-0 left-0 items-center justify-center"
        >
          <div
            class="built-by font-semibold text-gray-500 text-center text-xs sm:text-base"
          >
            Built by
            <a
              class="hover:text-indigo-700"
              href="https://steamcommunity.com/id/Switchy24/"
              >Switchy</a
            >
            &
            <a
              class="hover:text-indigo-700"
              href="https://steamcommunity.com/profiles/76561198054156585/"
              >Tottsiee</a
            >
          </div>
          <div
            class="hidden md:block cookie-disclaimer text-gray-400 text-xs m-2"
          >
            This website uses cookies, specifically google analytics. We don't
            store any information, we don't use any information. Purely just to
            see how active this site is.
          </div>
        </footer>
      </main>
    </div>
  </div>
</template>

<script>
const actions = {
  // appear: "Appears",  // Disabled for now
  bathBlood: "Bath filled with blood",
  bloodHandprints: "Bloody handprints",
  bloodPools: "Blood pools",
  bloodWriting: "Blood writing",
  blowsOutCandles: "Blows out candles",
  breathing: "Disembodied breathing",
  bruises: "Bruises the player (check arm)",
  chairCeiling: "Pins chair to ceiling",
  changeFlashlight: "Changes your flashlight",
  cockroaches: "Fill room with cockroaches",
  crucifixOutside: "Crucifixes outside",
  crucifixWalls: "Crucifixes on walls",
  crying: "Disembodied crying (not baby)",
  disableFlashlight: "Disables flashlight",
  duplicateItems: "Duplicate household objects",
  emptyBookcase: "Knocks books off of shelves",
  fakeHunt: "Fakes a hunt",
  flies: "Fill room with flies",
  floatingPainting: "Floating painting down hallways",
  graves: "Gravestones outside",
  hangDoll: "Hangs voodoo doll from ceiling",
  hissing: "Hissing",
  killPlants: "Kills plants",
  knockPaintings: "Knocks paintings off of walls",
  knocksDownFridge: "Knocks over the fridge",
  liftsAllObjects: "Lifts all objects",
  lockDoors: "Locks doors",
  moths: "Fill room with moths",
  moveDino: "Moves dino",
  moveKidsBall: "Move ball",
  opensDrawers: "Opens drawers",
  pentagrams: "Pentagrams on floor",
  rainBlood: "Raining blood",
  ringPhone: "Rings the phone",
  rockingChair: "Rocks the rocking chair",
  rollKidsBall: "Rolls ball",
  scribblePaintings: "Writing on paintings",
  sinkBlood: "Sink filled with blood",
  smashBottles: "Smashes bottles",
  smashPlates: "Smashes plates",
  smashWalls: "Smashes walls",
  smashWindow: "Smashes window (sound only)",
  spikeHR: "Spikes heart rate",
  stabWalls: "Stabs walls",
  stealItems: "Steal household items",
  stealPaintings: "Steals paintings",
  stealsRugs: "Steals rugs",
  stringUkelele: "Strings the ukelele",
  throwBall: "Throws ball",
  throwDino: "Throws dino",
  throwKnives: "Throws knives",
  toggleAlarm: "Turns on/off alarm",
  toggleBreaker: "Turns on/off breaker",
  toggleDoors: "Opens / closes doors",
  toggleKettle: "Turns on/off kettle",
  toggleLights: "Turns on/off lights",
  toggleOven: "Turns on/off oven",
  toggleRadio: "Turns on/off radio",
  toggleTV: "Turns on/off television",
  upsideDownPainting: "Paintings upside down",
  writingMirrors: "Writing on mirrors",
};

const exorcismItems = {
  candle: "Candle",
  crucifix: "Crucifix",
  rune: "Rune",
  water: "Holy Water",
  doll: "Possessed Doll",
  salt: "Salt",
};

export default {
  computed: {
    sortedActions() {
      let values = Object.values(actions);

      let sorted = values.sort();

      sorted = sorted.filter((a) =>
        a.toLowerCase().includes(this.filterActions.toLowerCase())
      );

      return sorted;
    },

    possibility() {
      if (this.selected.length === 0) {
        return [];
      }

      var possible = [];
      let ghosts = Object.keys(this.ghosts);

      for (var i = 0; i < ghosts.length; i++) {
        if (
          this.selected.every((selection) =>
            this.ghosts[ghosts[i]].actions.includes(selection)
          )
        ) {
          possible.push(ghosts[i]);
        }
      }

      return possible;
    },
  },

  data() {
    return {
      filterActions: "",
      checklist: false,
      selected: [],
      actions: actions,
      ghosts: {
        effigy: {
          exorcismItems: [exorcismItems.water, exorcismItems.rune],
          actions: [
            actions.throwKnives,
            actions.smashPlates,
            actions.stabWalls,
            actions.floatingPainting,
            actions.stealItems,
            actions.stringUkelele,
            actions.smashBottles,
            actions.rockingChair,
            actions.ringPhone,
            actions.emptyBookcase,
            actions.killPlants,
            actions.changeFlashlight,
            actions.graves,
            actions.crucifixWalls,
            actions.lockDoors,
            actions.blowsOutCandles,
            actions.toggleBreaker,
            actions.chairCeiling,
            actions.toggleLights,
            actions.toggleDoors,
            actions.toggleTV,
            actions.toggleRadio,
            actions.toggleKettle,
            actions.knockPaintings,
            actions.toggleAlarm,
            actions.throwDino,
            actions.rollKidsBall,
          ],
        },
        rusalka: {
          exorcismItems: [exorcismItems.water, exorcismItems.rune],
          actions: [
            actions.stabWalls,
            actions.floatingPainting,
            actions.stealItems,
            actions.writingMirrors,
            actions.stringUkelele,
            actions.rockingChair,
            actions.ringPhone,
            actions.emptyBookcase,
            actions.changeFlashlight,
            actions.lockDoors,
            actions.blowsOutCandles,
            actions.breathing,
            actions.toggleBreaker,
            actions.liftsAllObjects,
            actions.chairCeiling,
            actions.toggleDoors,
            actions.toggleOven,
            actions.toggleTV,
            actions.toggleRadio,
            actions.toggleKettle,
            actions.knockPaintings,
            actions.moveDino,
            actions.throwDino,
            actions.rollKidsBall,
          ],
        },
        demon: {
          exorcismItems: [exorcismItems.crucifix, exorcismItems.candle],
          actions: [
            actions.throwKnives,
            actions.stabWalls,
            actions.floatingPainting,
            actions.stealItems,
            actions.stringUkelele,
            actions.smashPlates,
            actions.smashBottles,
            actions.rockingChair,
            actions.ringPhone,
            actions.emptyBookcase,
            actions.changeFlashlight,
            actions.graves,
            actions.crucifixWalls,
            actions.crucifixOutside,
            actions.pentagrams,
            actions.lockDoors,
            actions.hissing,
            actions.toggleBreaker,
            actions.chairCeiling,
            actions.toggleLights,
            actions.toggleOven,
            actions.toggleTV,
            actions.toggleRadio,
            actions.knockPaintings,
            actions.toggleDoors,
            actions.scribblePaintings,
            actions.toggleAlarm,
            actions.throwDino,
            actions.moveKidsBall,
          ],
        },
        shade: {
          exorcismItems: [exorcismItems.water, exorcismItems.candle],
          actions: [
            actions.writingMirrors,
            actions.stringUkelele,
            actions.rockingChair,
            actions.ringPhone,
            actions.emptyBookcase,
            actions.blowsOutCandles,
            actions.breathing,
            actions.opensDrawers,
            actions.toggleLights,
            actions.toggleDoors,
            actions.toggleOven,
            actions.toggleTV,
            actions.toggleRadio,
            actions.toggleKettle,
            actions.knockPaintings,
            actions.toggleAlarm,
            actions.moveDino,
          ],
        },
        oni: {
          exorcismItems: [exorcismItems.crucifix, exorcismItems.doll],
          actions: [
            actions.throwKnives,
            actions.rainBlood,
            actions.floatingPainting,
            actions.bloodWriting,
            actions.stringUkelele,
            actions.smashPlates,
            actions.smashBottles,
            actions.rockingChair,
            actions.ringPhone,
            actions.emptyBookcase,
            actions.killPlants,
            actions.crucifixOutside,
            actions.lockDoors,
            actions.blowsOutCandles,
            actions.toggleBreaker,
            actions.chairCeiling,
            actions.toggleLights,
            actions.toggleOven,
            actions.toggleTV,
            actions.toggleRadio,
            actions.toggleKettle,
            actions.toggleDoors,
            actions.scribblePaintings,
            actions.toggleAlarm,
            actions.bruises,
            actions.moveDino,
            actions.throwBall,
            actions.knockPaintings,
            actions.stabWalls,
          ],
        },
        yurei: {
          exorcismItems: [exorcismItems.salt, exorcismItems.doll],
          actions: [
            actions.throwKnives,
            actions.flies,
            actions.sinkBlood,
            actions.floatingPainting,
            actions.writingMirrors,
            actions.bloodWriting,
            actions.smashPlates,
            actions.rockingChair,
            actions.ringPhone,
            actions.emptyBookcase,
            actions.graves,
            actions.crucifixOutside,
            actions.knocksDownFridge,
            actions.blowsOutCandles,
            actions.breathing,
            actions.opensDrawers,
            actions.liftsAllObjects,
            actions.toggleLights,
            actions.toggleKettle,
            actions.toggleAlarm,
            actions.smashWalls,
            actions.crying,
            actions.throwDino,
            actions.throwBall,
          ],
        },
        mare: {
          exorcismItems: [exorcismItems.crucifix, exorcismItems.rune],
          actions: [
            actions.moths,
            actions.cockroaches,
            actions.bathBlood,
            actions.sinkBlood,
            actions.bloodHandprints,
            actions.stringUkelele,
            actions.smashPlates,
            actions.smashBottles,
            actions.rockingChair,
            actions.ringPhone,
            actions.emptyBookcase,
            actions.killPlants,
            actions.changeFlashlight,
            actions.crucifixWalls,
            actions.knocksDownFridge,
            actions.blowsOutCandles,
            // actions.appear,
            actions.toggleTV,
            actions.toggleKettle,
            actions.knockPaintings,
            actions.upsideDownPainting,
            actions.stealsRugs,
            actions.stealPaintings,
            actions.bloodPools,
            actions.hangDoll,
            actions.scribblePaintings,
            actions.toggleAlarm,
            actions.smashWalls,
            actions.throwBall,
          ],
        },
        chimera: {
          exorcismItems: [exorcismItems.crucifix, exorcismItems.water],
          actions: [
            actions.stabWalls,
            actions.flies,
            actions.moths,
            actions.cockroaches,
            actions.sinkBlood,
            actions.bloodWriting,
            actions.bloodHandprints,
            actions.smashPlates,
            actions.smashBottles,
            actions.rockingChair,
            actions.ringPhone,
            actions.disableFlashlight,
            actions.fakeHunt,
            actions.pentagrams,
            actions.knocksDownFridge,
            actions.duplicateItems,
            actions.toggleBreaker,
            actions.toggleLights,
            actions.toggleDoors,
            actions.toggleOven,
            actions.toggleTV,
            actions.toggleKettle,
            actions.toggleAlarm,
            actions.smashWindow,
            actions.spikeHR,
            actions.moveDino,
          ],
        },
      },
    };
  },

  methods: {
    selectAction(a) {
      if (this.selected.includes(a)) {
        let index = this.selected.indexOf(a);
        this.selected.splice(index, 1);
      } else {
        this.selected.push(a);
      }
    },

    isSelected(a) {
      if (this.selected.includes(a)) return true;

      return false;
    },

    ucFirst(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    },

    getItems(ghost) {
      const arr = this.ghosts[ghost].exorcismItems;

      return `${arr[0]} & ${arr[1]}`;
    },
  },
};
</script>
