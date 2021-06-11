<template>
  <div>
    <div class="oa-modal">
      <div>
        <header>{{ message.title }}</header>
        <main>{{ message.message }}</main>
        <footer>
          <button @click="message.buttons.ok.callback">
            {{ message.buttons.ok.label }}
          </button>
          <button
            :disabled="buttonCancelDisabled"
            v-on="
              buttonCancelDisabled
                ? {}
                : { click: () => message.buttons.cancel.callback() }
            "
          >
            {{ message.buttons.cancel.label }}
          </button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      buttonCancelDisabled: true,
      //can't use 'default' because it's a JS restricted word
      message: this.getDefaultMessage(),
    };
  },
  methods: {
    getDefaultMessage() {
      return {
        title: "ALERT",
        message: "You need to define you're custom message...",
        buttons: {
          ok: {
            label: "OK",
            callback: this.switchModal,
          },
          cancel: {
            label: "CANCEL",
            callback: this.switchModal,
          },
        },
      };
    },

    show(obj) {
      //set message default values
      this.message = this.getDefaultMessage();
      this.buttonCancelDisabled = true;
      if ("message" in obj) {
        this.message.message = obj.message;
        if ("title" in obj) {
          this.message.title = obj.title;
        }
        //button OK
        if ("buttons" in obj && "ok" in obj.buttons) {
          //check if custom label exists
          if ("label" in obj.buttons.ok) {
            this.message.buttons.ok.label = obj.buttons.ok.label;
          }
          //check if callback is defined
          if (
            obj.buttons.ok.callback !== undefined &&
            typeof obj.buttons.ok.callback === "function"
          ) {
            //adding callback to click action
            this.message.buttons.ok.callback = () => {
              this.switchModal();
              obj.buttons.ok.callback();
            };
          }
        }

        //button CANCEL
        if ("buttons" in obj && "cancel" in obj.buttons) {
          this.buttonCancelDisabled = false;
          //check if custom label exists
          if ("label" in obj.buttons.cancel) {
            this.message.buttons.cancel.label = obj.buttons.cancel.label;
          }
          if (
            "callback" in obj.buttons.cancel &&
            typeof obj.buttons.cancel.callback === "function"
          ) {
            //adding callback to click action
            this.message.buttons.cancel.callback = () => {
              this.switchModal();
              obj.buttons.cancel.callback();
            };
          }
        }

        //open dialog
        this.switchModal();
      } else {
        console.log("You need to define a message to show!");
      }
    },

    switchModal() {
      document.querySelector(".oa-modal").classList.toggle("active");
    },
  },
};
</script>

<style scoped>
.oa-modal {
  position: fixed;
  display: flex;
  z-index: 1000;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  justify-content: center;
  flex-direction: row;
  visibility: hidden;
  opacity: 0;
  transform: scale(0, 0);
  transition: all 0.15s ease-out;
}
.oa-modal.active {
  visibility: visible;
  opacity: 1;
  transform: scale(1, 1);
}
.oa-modal > div {
  background-color: #252525;
  color: #fff;
  align-self: center;
  padding: 1rem;
  border-radius: 0.4rem;
  box-shadow: 0 0 0.4rem #222;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 1rem;
}
.oa-modal > div > header {
  text-align: left;
  font-weight: 700;
}
.oa-modal > div > main {
  margin: 1rem 0;
  max-width: 320px;
  text-align: left;
}
.oa-modal > div > footer {
  display: flex;
  flex-direction: row-reverse;
}
.oa-modal > div > footer > button {
  margin-left: 0.5rem;
  border: none;
  background-color: transparent;
  color: #fff;
  cursor: pointer;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 1rem;
  border-radius: 0.4rem;
  transition: color 0.4s;
  font-weight: 700;
}
.oa-modal > div > footer > button:hover {
  color: #aaa;
}
.oa-modal > div > footer > button:disabled {
  display: none;
}
</style>
