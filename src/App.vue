<template>
  <div id="app">
    <div class="flex flex-row max-h-screen">
      <aside class="flex flex-col w-1/3 border-r">
        <nav
          class="flex-1 min-h-0 overflow-y-auto bg-indigo-200"
          aria-label="List of ghost actions"
        >
          <ul>
            <li
              class="p-2 border-b border-indigo-300 cursor-pointer"
              :class="{ 'bg-gray-200': isSelected(a) }"
              v-for="a in actions"
              :key="a"
              @click="selectAction(a)"
            >
              <div class="flex flex-row justify-between items-center">
                <h3 class="text-3xl">{{ a }}</h3>
              </div>
            </li>
          </ul>
        </nav>
      </aside>
      <main class="flex flex-col w-2/3 relative">
        <div class="flex flex-col flex-grow items-center justify-center">
          <h1 class="text-2xl text-gray-600">Your ghost could be...</h1>
          <h1 class="text-4xl" v-for="p in possibility" :key="p">{{ p }}</h1>
        </div>

        <footer
          class="flex w-full h-16 absolute bottom-0 left-0 items-center justify-center font-semibold text-gray-500"
        >
          Built by
          <a
            class="m-1 hover:text-indigo-700"
            href="https://steamcommunity.com/id/Switchy24/"
            >Switchy</a
          >
          &
          <a
            class="m-1 hover:text-indigo-700"
            href="https://steamcommunity.com/profiles/76561198054156585/"
            >Tottsiee</a
          >
        </footer>
      </main>
    </div>
  </div>
</template>

<script>
const actions = {
  throwKnives: "Throw Knives",
  stabWalls: "Stabs Walls",
  flies: "Fill room with flies",
  moths: "Fill room with moths",
  cockroaches: "Fill room with cockroaches",
  bathBlood: "Bath filled with blood",
  sinkBlood: "Sink filled with blood",
  rainBlood: "Raining blood",
  floatingPainting: "Floating painting down hallways",
  stealItems: "Steal household items",
  writingMirrors: "Writing on mirrors",
  bloodWriting: "Blood writing",
  bloodHandprints: "Bloody Handprints",
  stringUkelele: "Strings the ukelele",
  smashPlates: "Smashes plates",
  smashBottles: "Smashes bottles",
  rockingChair: "Rocks the rocking chair",
  ringPhone: "Rings the phone",
  emptyBookcase: "Knocks books off of shelves",
  killPlants: "Kills plants",
  changeFlashlight: "Changes your flashlight",
  graves: "Gravestones outside",
  crucifixWalls: "Crucifixes on walls",
  crucifixOutside: "Crucifixes outside",
  disableFlashlight: "Disables flashlight",
  fakeHunt: "Fakes a hunt",
  pentagrams: "Pentagrams on floor",
  lockDoors: "Locks doors",
  knocksDownFridge: "Knocks over the fridge",
  duplicateItems: "Duplicate household objects",
  blowsOutCandles: "Blows out candles",
  breathing: "Disembodied Breathing",
  hissing: "Hissing",
  breakerOff: "Turns off breaker",
  toggleBreaker: "Turn on/off breaker",
  openDrawers: "Opens drawers",
  liftsAllObjects: "Lifts all objects",
  appear: "Appears",
  chairCeiling: "Pins chair to ceiling",
  toggleLights: "Turns on/off lights",
  toggleDoors: "Opens/closes doors",
  toggleOven: "Turns on/off oven",
  toggleTV: "Turns on/off television",
  toggleRadio: "Turns on/off radio",
  toggleKettle: "Turns on/off kettle",
  kettleOn: "Turns on kettle",
  knockPaintings: "Knocks paintings off of walls",
  upsideDownPainting: "Paintings upside down.",
  openDoors: "Opens doors",
  scribblePaintings: "Writes/Scribbles on paintings",
  stealsRugs: "Steals rugs",
  stealPaintings: "Steals paintings",
  bloodPools: "Blood pools",
  hangDoll: "Hangs voodoo doll from ceiling",
  toggleAlarm: "Turns on/off alarm",
  smashWalls: "Smashes walls",
  smashWindow: "Smashes window (sound only)",
  crying: "Disembodied crying (not baby)",
  spikeHR: "Spikes heart rate",
  bruises: "Bruises the player (check arm)",
  throwDino: "Throws dino",
  moveDino: "Moves dino",
  movesKidsToys: "Moves kids toys",
  throwsKidsToys: "Throws kids toys",
  throwBall: "Throws footballs",
  throwsKidsBall: "Throws kids ball",
  rollKidsBall: "Rolls kids ball",
  moveKidsBall: "Move kids ball",
  playKidsBall: "Plays with kids ball",
};

export default {
  computed: {
    possibility() {
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
