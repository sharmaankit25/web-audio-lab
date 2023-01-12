<script setup lang="ts">
import * as Tone from "tone";

const audioR = new Tone.Oscillator(80);
const audioL = new Tone.Oscillator(90);

function playSound() {
  //create a synth and connect it to the main output (your speakers)
  const synth = new Tone.Synth().toDestination();
  const now = Tone.now();
  //play a middle 'C' for the duration of an 8th note
  synth.triggerAttackRelease("C3", "4n");
  synth.triggerAttackRelease("E3", "8n", now + 0.5);
  synth.triggerAttackRelease("G4", "8n", now + 1);
  // synth.triggerAttack("D4", now);
  // synth.triggerAttack("F4", now + 0.5);
  // synth.triggerAttack("A4", now + 1);
  // synth.triggerAttack("C5", now + 1.5);
  // synth.triggerAttack("E5", now + 2);
  // synth.triggerRelease(["D4", "F4", "A4", "C5", "E5"], now + 4);
}

function playDeltaWave(action: string) {
  audioR.disconnect();
  audioL.disconnect();
  if (action === "stop") {
    audioR.stop();
    audioL.stop();
  }
  if (action === "start") {
    const panner = new Tone.Panner(0);
    const autoPanner = new Tone.AutoPanner("1n").toDestination().start();
    audioR.connect(panner.pan.rampTo(-1));
    audioR.start();
    audioL.connect(panner.pan.rampTo(1)).connect(autoPanner);
    audioL.start();
  }
}
</script>

<template>
  <button class="button" @mousedown="playSound">Test Sound</button>
  <button @mousedown="playDeltaWave('start')">Play Delta Wave</button>
  <button @click="playDeltaWave('stop')">Stop</button>
</template>
