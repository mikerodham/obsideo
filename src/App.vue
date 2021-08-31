<template>
  <div id="app" class="bg-gray-50">
    <div class="flex flex-row max-h-screen overflow-hidden">
      <aside class="flex flex-col w-1/2 sm:w-1/3 border-r bg-indigo-200">
        <nav
          class="flex-1 min-h-0 overflow-y-auto"
          aria-label="List of ghost actions"
        >
          <ul>
            <li
              class="p-2 border-b border-indigo-300 cursor-pointer"
              :class="{ 'bg-gray-200': isSelected(a) }"
              v-for="a in sortedActions"
              :key="a"
              @click="selectAction(a)"
            >
              <div class="flex flex-row justify-between items-center">
                <h3 class="sm:text-xl md:text-2xl">{{ a }}</h3>
              </div>
            </li>
          </ul>
        </nav>
      </aside>
      <main class="flex flex-col w-1/2 sm:w-2/3 relative">
        <div
          class="flex flex-col sm:flex-row w-full absolute justify-between p-4"
        >
          <button
            class="block text-white px-4 py-2 uppercase font-semibold mr-4 text-xs md:text-base mb-4 sm:mb-0"
            :class="{
              'bg-gray-700 cursor-not-allowed': selected.length === 0,
              'bg-green-700': selected.length > 0,
            }"
            :disabled="selected.length === 0"
            @click="selected = []"
          >
            Reset Selection
          </button>

          <div class="hidden sm:flex flex-col sm:flex-row">
            <a
              class="text-center text-white px-4 py-2 uppercase font-semibold steam mr-4 text-xs md:text-base"
              href="https://store.steampowered.com/app/1708460/Obsideo/"
            >
              Steam Store
            </a>

            <a
              class="text-center text-white px-4 py-2 uppercase font-semibold discord mr-4 sm:mr-4 md:mr-0 text-xs md:text-base"
              href="https://discord.gg/obsideo"
            >
              Join Discord
            </a>
          </div>
        </div>
        <div class="flex flex-col flex-grow items-center justify-center">
          <h2
            class="sm:text-xl md:text-2xl text-gray-600 mb-4"
            v-if="selected.length > 0 && this.possibility.length > 0"
          >
            Your ghost could be...
          </h2>
          <h2
            class="sm:text-xl md:text-2xl text-gray-600 mb-4 px-8 text-center"
            v-if="selected.length === 0"
          >
            Select an evidence first and your ghost types will appear here.
          </h2>

          <h2
            class="sm:text-xl md:text-2xl text-gray-600 mb-4 px-8 text-center"
            v-if="selected.length > 0 && this.possibility.length === 0"
          >
            We could not find a match.
            <span>
              This could be you've entered something wrong, due to a bug with
              our website or due to a bug with the game.
            </span>
          </h2>
          <h1
            class="text-xl sm:text-2xl md:text-4xl"
            v-for="p in possibility"
            :key="p"
          >
            {{ p }}
          </h1>
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
  appear: "Appears",
  bathBlood: "Bath filled with blood",
  bloodHandprints: "Bloody handprints",
  bloodPools: "Blood pools",
  bloodWriting: "Blood writing",
  blowsOutCandles: "Blows out candles",
  breakerOff: "Turns off breaker",
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
  kettleOn: "Turns on kettle",
  killPlants: "Kills plants",
  knockPaintings: "Knocks paintings off of walls",
  knocksDownFridge: "Knocks over the fridge",
  liftsAllObjects: "Lifts all objects",
  lockDoors: "Locks doors",
  moths: "Fill room with moths",
  moveDino: "Moves dino",
  moveKidsBall: "Move kids ball",
  movesKidsToys: "Moves kids toys",
  openDoors: "Opens doors",
  opensDrawers: "Opens drawers",
  pentagrams: "Pentagrams on floor",
  playKidsBall: "Plays with kids ball",
  rainBlood: "Raining blood",
  ringPhone: "Rings the phone",
  rockingChair: "Rocks the rocking chair",
  rollKidsBall: "Rolls kids ball",
  scribblePaintings: "Writes / Scribbles on paintings",
  sinkBlood: "Sink filled with blood",
  smashBottles: "Smashes bottles",
  smashPlates: "Smashes plates",
  smashWalls: "Smashes walls",
  smashWindow: "Smashes window (sound only)",
  spikeHR: "Spikes heart rate",
  stabWalls: "Stabs Walls",
  stealItems: "Steal household items",
  stealPaintings: "Steals paintings",
  stealsRugs: "Steals rugs",
  stringUkelele: "Strings the ukelele",
  throwBall: "Throws footballs",
  throwDino: "Throws dino",
  throwKnives: "Throw Knives",
  throwsKidsBall: "Throws kids ball",
  throwsKidsToys: "Throws kids toys",
  toggleAlarm: "Turns on/off alarm",
  toggleBreaker: "Turn on/off breaker",
  toggleDoors: "Opens / closes doors",
  toggleKettle: "Turns on/off kettle",
  toggleLights: "Turns on/off lights",
  toggleOven: "Turns on/off oven",
  toggleRadio: "Turns on/off radio",
  toggleTV: "Turns on/off television",
  upsideDownPainting: "Paintings upside down",
  writingMirrors: "Writing on mirrors",
};

export default {
  computed: {
    sortedActions() {
      let values = Object.values(actions);

      let sorted = values.sort();
      return sorted;
    },

    possibility() {
      if (this.selected.length === 0) {
        return null;
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
      selected: [],
      actions: actions,
      ghosts: {
        effigy: {
          actions: [
            actions.throwKnives,
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
            actions.throwsKidsToys,
            actions.rollKidsBall,
          ],
        },
        rusalka: {
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
            actions.breakerOff,
            actions.liftsAllObjects,
            actions.chairCeiling,
            actions.toggleDoors,
            actions.toggleOven,
            actions.toggleTV,
            actions.toggleRadio,
            actions.toggleKettle,
            actions.knockPaintings,
            actions.movesKidsToys,
            actions.throwsKidsToys,
            actions.playKidsBall,
          ],
        },
        demon: {
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
            actions.openDoors,
            actions.scribblePaintings,
            actions.toggleAlarm,
            actions.throwsKidsToys,
            actions.moveKidsBall,
          ],
        },
        shade: {
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
            actions.movesKidsToys,
          ],
        },
        oni: {
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
            actions.openDoors,
            actions.scribblePaintings,
            actions.toggleAlarm,
            actions.bruises,
            actions.movesKidsToys,
            actions.throwsKidsBall,
            actions.knockPaintings,
            actions.stabWalls
          ],
        },
        yurei: {
          actions: [
            actions.throwKnives,
            actions.flies,
            actions.sinkBlood,
            actions.floatingPainting,
            actions.writingMirrors,
            actions.bloodWriting,
            actions.bloodHandprints,
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
            actions.kettleOn,
            actions.toggleAlarm,
            actions.smashWalls,
            actions.crying,
            actions.throwDino,
            actions.throwBall,
          ],
        },
        mare: {
          actions: [
            actions.moths,
            actions.cockroaches,
            actions.bathBlood,
            actions.sinkBlood,
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
            actions.appear,
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
  },
};
</script>
