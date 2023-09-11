<template>
  <div style="text-align: left">
    <span> {{ displayStr }} </span>{{ cursorStr }}
  </div>
</template>
<script>
import { ref } from "vue";
export default {
  name: "TypeWriter",
  props: {
    text: {
      type: [String, Array],
      required: true,
    },
    speed: {
      type: Number,
      default: 100,
    },
    waitAfterWrite: {
      type: Number,
      default: 1500,
    },
    waitAfterDelete: {
      type: Number,
      default: 300,
    },
    /**
     * Prop for multi-word typewriter. Defines whether each word is
     * wholly deleted or character by character after completion of write.
     */
    delete: {
      type: String,
      validator(value) {
        return ["full", "type"].includes(value);
      },
      required: true,
    },
    /**
     * Prop to add randomization to typing speed for more 'human' typing.
     */
    humanize: {
      type: Boolean,
      default: true,
    },
    /**
     * Defines whether typewriter repeats. If false, emits done when finished
     */
    repeats: {
      type: Boolean,
      default: false,
    },
    /**
     * Add cursor
     */
    cursor: {
      type: Boolean,
      default: false,
    },
  },
  emits: ["done"],
  setup(props, { emit }) {
    const displayStr = ref("");
    const cursorStr = ref(" ");
    var i = 0;
    var counter = 0;
    var flip = false;

    const blinkGap = 450;
    let blinkTimeElapsed = 0;
    const cursorBlink = (timeToBlink) => {
      if (blinkTimeElapsed < timeToBlink) {
        if (cursorStr.value === "_") {
          cursorStr.value = " ";
        } else {
          cursorStr.value = "_";
        }
        blinkTimeElapsed += blinkGap;
        setTimeout(() => {
          cursorBlink(timeToBlink);
        }, blinkGap);
      } else {
        blinkTimeElapsed = 0;
        cursorStr.value = "_";
        multiTypeWriter();
      }
    };

    const multiTypeWriter = () => {
      const currString = Array.isArray(props.text)
        ? props.text[counter]
        : props.text;

      if (!flip && i < currString.length) {
        // add next character in string
        displayStr.value = displayStr.value + currString[i];
        i++;
        if (i === currString.length) {
          // if we've printed all the characters in the current string, flip direction
          flip = true;
          cursorBlink(props.waitAfterWrite);
        } else {
          setTimeout(() => {
            multiTypeWriter();
          }, props.speed + (props.humanize ? Math.random() * 100 : 0));
        }
      } else if (props.delete === "type" && i > -1) {
        // remove characters at the end one at a time.
        displayStr.value = displayStr.value.substring(
          0,
          displayStr.value.length - 1
        );
        i--;
        setTimeout(() => {
          multiTypeWriter();
        }, props.speed + (props.humanize ? Math.random() * 100 : 1));
      } else {
        // delete entire typed string immediately.
        displayStr.value = "";
        i = 0;
        flip = false;
        if (Array.isArray(props.text)) {
          counter++;
          if (counter === props.text.length) {
            counter = 0;
          }
        }
        if (props.repeats || counter !== 0) {
          cursorBlink(props.waitAfterDelete);
        } else {
          emit("done");
        }
      }
    };

    cursorBlink(props.waitAfterWrite);

    return {
      displayStr,
      cursorStr,
    };
  },
};
</script>
