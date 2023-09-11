<template>
  <span>
    {{ displayStr }}
  </span>
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
      default: 1000,
    },
    waitAfterDelete: {
      type: Number,
      default: 200,
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
  },
  emits: ["done"],
  setup(props, { emit }) {
    const displayStr = ref("");
    var i = 0;
    var counter = 0;
    var flip = false;

    const multiTypeWriter = () => {
      const currString = Array.isArray(props.text)
        ? props.text[counter]
        : props.text;

      if (!flip && i < currString.length) {
        // add next character in string
        displayStr.value =
          displayStr.value.substring(0, displayStr.value.length - 1) +
          currString[i] +
          "_";
        i++;
        if (i === currString.length) {
          // if we've printed all the characters in the current string, flip direction
          flip = true;
          setTimeout(() => {
            multiTypeWriter();
          }, props.waitAfterWrite);
        } else {
          setTimeout(() => {
            multiTypeWriter();
          }, props.speed + (props.humanize ? Math.random() * 100 : 0));
        }
      } else if (props.delete === "type" && i > -1) {
        // remove characters at the end one at a time.
        displayStr.value =
          displayStr.value.substring(0, displayStr.value.length - 2) + "_";
        i--;
        setTimeout(() => {
          multiTypeWriter();
        }, props.speed + (props.humanize ? Math.random() * 100 : 1));
      } else {
        // delete entire typed string immediately.
        displayStr.value = "_";
        i = 0;
        flip = false;
        counter++;
        if (Array.isArray(props.text) && counter === props.text.length) {
          counter = 0;
        }
        if (props.repeats || counter !== 0) {
          setTimeout(() => {
            multiTypeWriter();
          }, props.waitAfterDelete);
        } else {
          emit("done");
        }
      }
    };
    setTimeout(() => {
      multiTypeWriter();
    }, props.waitAfterWrite);
    return {
      displayStr,
      multiTypeWriter,
    };
  },
};
</script>
