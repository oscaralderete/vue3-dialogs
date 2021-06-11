<template>
  <div id="app">
    <h1>Basic Vue3 dialog + toast component</h1>

    <!-- toast -->
    <h3>Toast from top to bottom</h3>
    <button @click="callToast(1)">Call toast</button>&nbsp;
    <button @click="callToast(1, 'success')">Call success toast</button>&nbsp;
    <button @click="callToast(1, 'error')">Call error toast</button>&nbsp;
    <button @click="callToast(1, 'warning')">Call warning toast</button>
    <br />
    <h3>Toast from bottom to top</h3>
    <button @click="callToast(0)">Call toast</button>&nbsp;
    <button @click="callToast(0, 'success')">Call success toast</button>&nbsp;
    <button @click="callToast(0, 'error')">Call error toast</button>&nbsp;
    <button @click="callToast(0, 'warning')">Call warning toast</button>
    <br />
    <h3>Try toast component:</h3>
    <div class="form-container">
      <form class="pure-form pure-form-stacked" @submit.prevent>
        <fieldset>
          <label>Your toast message (always required):</label>
          <input
            type="text"
            ref="ct_message"
            v-model="ct.message"
            class="pure-input-1"
            placeholder="Your custom message here"
          />
          <label>Type:</label>
          <select v-model="ct.type" class="pure-input-1">
            <option v-for="(i, index) in types" :value="i.value" :key="index">
              {{ i.label }}
            </option>
          </select>
          <label>Position:</label>
          <select v-model="ct.position" class="pure-input-1">
            <option
              v-for="(i, index) in positions"
              :value="i.value"
              :key="index"
            >
              {{ i.label }}
            </option>
          </select>
          <label
            ><input
              type="checkbox"
              v-model="enableToastCallback"
              value="true"
            />
            Run callback after toast ends (you can code your own callback, in
            this case it will reset this form)</label
          >
          <button class="pure-button pure-button-primary" @click="customToast">
            Launch custom toast
          </button>
        </fieldset>
      </form>
    </div>
    <hr />
    <br />

    <!-- modal -->
    <button @click="runPresetModal">Call modal with preset data</button>

    <h3>Try modal component:</h3>
    <div class="form-container">
      <form class="pure-form pure-form-stacked" @submit.prevent>
        <fieldset>
          <label>Message (the only required field):</label>
          <input
            type="text"
            class="pure-input-1"
            ref="mo_message"
            v-model="mo.message"
            placeholder="Modal message (the only required field)"
          />
          <label>Message title (by default "ALERT"):</label>
          <input
            type="text"
            class="pure-input-1"
            ref="mo_title"
            v-model="mo.title"
            placeholder="Custom title of your dialog"
            maxlength="20"
          />
          <label>OK button label (by default "OK"):</label>
          <input
            type="text"
            class="pure-input-1"
            v-model="mo.ok_label"
            maxlength="15"
            placeholder="By default: OK"
          />
          <label>OK button callback:</label>
          <label
            ><input
              type="checkbox"
              value="true"
              v-model="enableModalOkCallback"
            />
            Run callback after click on OK button (you can code your own
            callback, in this case it will just log)</label
          >
          <ul class="logs" v-if="ok_logs.length > 0">
            <li v-for="(i, index) in ok_logs" :key="index">{{ i }}</li>
          </ul>
          <label>CANCEL button label (by default "CANCEL"):</label>
          <input
            type="text"
            class="pure-input-1"
            v-model="mo.cancel_label"
            maxlength="15"
            placeholder="By default: CANCEL"
          />
          <label>CANCEL button callback:</label>
          <label
            ><input
              type="checkbox"
              value="true"
              v-model="enableModalCancelCallback"
            />
            Run callback after click on CANCEL button (you can code your own
            callback, in this case it will just log)</label
          >
          <ul class="logs" v-if="cancel_logs.length > 0">
            <li v-for="(i, index) in cancel_logs" :key="index">{{ i }}</li>
          </ul>
          <br />
          <button @click="customModal" class="pure-button pure-button-primary">
            Launch custom modal
          </button>
        </fieldset>
      </form>
    </div>

    <Toast ref="myToast" />
    <Modal ref="myModal" />
  </div>
</template>

<script>
import Modal from "./components/Modal";
import Toast from "./components/Toast";

export default {
  name: "App",
  data() {
    return {
      showModal: false,
      showToast: false,
      customMsg: "",
      modal: {
        title: "WARNING",
        message: "This is a customized message",
        buttons: {
          ok: {
            label: "OK",
            callback: this.handleOK,
          },
          cancel: {
            label: "CANCEL",
            callback: this.handleCancel,
          },
        },
      },
      ct: {
        message: "",
        type: "",
        position: "",
      },
      types: [
        { value: "", label: "Default" },
        { value: "error", label: "Error" },
        { value: "success", label: "Success" },
        { value: "warning", label: "Warning" },
      ],
      positions: [
        { value: "", label: "Bottom" },
        { value: "top", label: "Top" },
      ],
      enableToastCallback: false,
      mo: {
        message: "",
        title: "",
        ok_label: "",
        cancel_label: "",
      },
      enableModalOkCallback: false,
      enableModalCancelCallback: false,
      ok_logs: [],
      cancel_logs: [],
    };
  },
  components: { Modal, Toast },
  methods: {
    handleOK() {
      console.log("Parent controller says OK button has been clicked");
    },
    handleCancel() {
      console.log("Parent controller says CANCEL button has been clicked");
    },
    runPresetModal() {
      this.$refs.myModal.show({
        title: "ALERT",
        message: "You called the configurable modal component!",
        buttons: {
          ok: {
            label: "ACCEPT",
            callback: this.handleOK,
          },
          cancel: {
            label: "DENY",
            callback: this.handleCancel,
          },
        },
      });
    },
    customModal() {
      if (this.mo.message.trim() === "") {
        this.$refs.myModal.show({
          title: "ERROR",
          message: "Your modal message can't be empty!",
          buttons: {
            ok: {
              callback: () => {
                console.log("Default modal callback..");
                this.$refs.mo_message.focus();
              },
            },
          },
        });
        return false;
      }
      const x = {};
      x.message = this.mo.message;
      if (this.mo.title.trim() !== "") {
        x.title = this.mo.title;
      }
      //OK button
      if (this.mo.ok_label.trim() !== "") {
        x.buttons = {};
        x.buttons.ok = {};
        x.buttons.ok.label = this.mo.ok_label;
      }
      if (this.enableModalOkCallback) {
        //OK button callback enabled
        if ("buttons" in x) {
          if ("ok" in x.buttons) {
            x.buttons.ok.callback = () => {
              this.log(0, "OK has been pressed");
            };
          }
        } else {
          x.buttons = {};
          x.buttons.ok = {};
          x.buttons.ok.callback = () => {
            this.log(0, "OK has been pressed");
          };
        }
      }
      //CANCEL button
      if (this.mo.cancel_label.trim() !== "") {
        if ("buttons" in x) {
          //nothing...
        } else {
          x.buttons = {};
        }
        x.buttons.cancel = {};
        x.buttons.cancel.label = this.mo.cancel_label;
        console.log("cancel_label is defined");
      }
      if (this.enableModalCancelCallback) {
        //OK button callback enabled
        if ("buttons" in x) {
          if ("cancel" in x.buttons) {
            x.buttons.cancel.callback = () => {
              this.log(1, "CANCEL has been pressed");
            };
          } else {
            x.buttons.cancel = {};
            x.buttons.cancel.callback = () => {
              this.log(1, "CANCEL has been pressed");
            };
          }
        } else {
          x.buttons = {};
          x.buttons.cancel = {};
          x.buttons.cancel.callback = () => {
            this.log(1, "CANCEL has been pressed");
          };
        }
      }
      console.log("modal obj:", x);
      this.$refs.myModal.show(x);
    },
    //toast
    callToast(num, str) {
      const toast = {
        title: "Click to close",
        message: "Default toast message set from parent",
        callback: this.handleToast,
      };
      if (num > 0) {
        toast.position = "top";
      }
      if (str !== undefined) {
        switch (str) {
          case "success":
          case "error":
          case "warning":
            toast.type = str;
            toast.message = `This is a${
              str === "error" ? "n" : ""
            } "${str}" type message set from parent`;
        }
      }
      this.$refs.myToast.text(toast);
    },
    handleToast() {
      console.log("Callback set from parent has been called!");
    },
    customToast() {
      console.log(this.enableToastCallback);
      if (this.ct.message === "") {
        this.$refs.myToast.text({
          message: "You need enter your custom message!",
          type: "error",
          callback: this.focusOnToastMessage,
        });
        return false;
      }
      const x = {
        message: this.ct.message,
        type: this.ct.type,
        position: this.ct.position,
        callback: () => {
          if (this.enableToastCallback) {
            this.ct.message = "";
            this.ct.type = "";
            this.ct.position = "";
          } else {
            //nothing
          }
        },
      };
      this.$refs.myToast.text(x);
    },
    focusOnToastMessage() {
      this.$refs.ct_message.focus();
    },
    //log
    log(num, str) {
      const x = new Date();
      str = `[${x.getHours()}:${x.getMinutes()}:${x.getSeconds()}] Button callback. ${str}`;
      if (num === 0) {
        this.ok_logs.push(str);
      } else {
        this.cancel_logs.push(str);
      }
    },
  },
};
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  box-sizing: border-box;
  padding: 1em;
}
br {
  content: "A" !important;
  display: block !important;
  margin-bottom: 1.5em !important;
}
.form-container {
  text-align: left;
}
input[type="text"]:focus {
  background-color: #ff9 !important;
}
ul.logs {
  list-style-type: none;
  background-color: #000;
  color: #fff;
  padding: 0.5em 0;
  margin: 0;
  max-height: 10em;
  overflow-y: auto;
  display: flex;
  flex-direction: column-reverse;
}
ul.logs > li {
  padding: 0.5em 1em;
  font-family: "Courier New", Courier, monospace;
}
</style>
