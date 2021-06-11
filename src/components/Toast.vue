<template>
  <div>
    <div
      class="oa-toast oa-toast-top"
      @click="top_toast.callback"
      :title="top_toast.title"
    >
      <section :class="top_toast.type">
        {{ top_toast.message }}
      </section>
    </div>
    <div
      class="oa-toast oa-toast-bottom"
      @click="bottom_toast.callback"
      :title="bottom_toast.title"
    >
      <section :class="bottom_toast.type">
        {{ bottom_toast.message }}
      </section>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      toastDuration: 5, //in seconds
      top_toast: this.getDefaultToast(),
      bottom_toast: this.getDefaultToast(),
    };
  },
  methods: {
    getDefaultToast() {
      return {
        title: "Click to close toast",
        message: "Put your message here",
        type: "",
        callback: () => console.log("callback here.."),
        timeout: null,
      };
    },
    text(obj) {
      const p =
        "position" in obj
          ? obj.position === "top"
            ? "top"
            : "bottom"
          : "bottom";
      const el = document.querySelector(".oa-toast-" + p);
      clearTimeout(this[p + "_toast"].timeout);
      this[p + "_toast"].timeout = null;

      //when toast is active
      if (el.classList.contains("active")) {
        //remove toast
        setTimeout(() => {
          this.text(obj);
        }, 200);
      } else {
        //set toast content
        this.setToast(obj);
        this[p + "_toast"].timeout = setTimeout(() => {
          el.classList.remove("active");
          if ("callback" in obj && typeof obj.callback === "function") {
            try {
              obj.callback();
            } catch (er) {
              console.log("Callback error", er);
            }
          }
        }, this.toastDuration * 1000);
        //el.classList.add("active");
      }
      el.classList.toggle("active");
    },
    setToast(obj) {
      const p = "position" in obj && obj.position === "top" ? "top" : "bottom";
      //message always must be defined
      this[p + "_toast"].message = obj.message;
      if ("type" in obj && typeof obj.type === "string") {
        this[p + "_toast"].type = obj.type;
      } else {
        this[p + "_toast"].type = "";
      }
      if ("title" in obj && typeof obj.title === "string") {
        this[p + "_toast"].title = obj.title;
      }
      if ("callback" in obj && typeof obj.callback === "function") {
        this[p + "_toast"].callback = () => {
          this.killToast(p);
          try {
            obj.callback();
          } catch (er) {
            console.log("Callback error", er);
          }
        };
      } else {
        this[p + "_toast"].callback = () => this.killToast(p);
      }
    },
    killToast(str) {
      clearTimeout(this[str + "_toast"].timeout);
      this[str + "_toast"].timeout = null;
      document.querySelector(".oa-toast-" + str).classList.remove("active");
    },
  },
};
</script>

<style scoped>
/* toast */
.oa-toast {
  position: fixed;
  display: flex;
  left: 50%;
  justify-content: center;
  width: auto;
  background-color: transparent;
  transform: translateX(-50%);
  opacity: 0;
  visibility: hidden;
  transition: all 0.1s;
}

.oa-toast.oa-toast-top {
  top: 0;
}

.oa-toast.oa-toast-bottom {
  bottom: 0;
}

/* section */
.oa-toast > section {
  background-color: #34495e;
  color: #fff;
  align-self: center;
  box-sizing: border-box;
  padding: 1rem;
  box-shadow: 0 0 0.4rem #222;
  border-radius: 0.4rem;
  cursor: pointer;
  text-align: left;
}

.oa-toast > section.success {
  background-color: #2dcc70;
}
.oa-toast > section.error {
  background-color: #e54c3c;
}
.oa-toast > section.warning {
  background-color: #f1c30e;
}

/* toast active */
.oa-toast.oa-toast-top.active {
  top: 1em;
  opacity: 1;
  visibility: visible;
  transition: all 0.3s ease;
}
.oa-toast.oa-toast-bottom.active {
  bottom: 1em;
  opacity: 1;
  visibility: visible;
  transition: all 0.4s ease-in;
}

/*.oa-toast > section > div {
  display: block;
  margin-top: 0.5rem;
  height: 0.2rem;
  background-color: transparent;
  position: relative;
}
.oa-toast > section > div > div {
  background-color: #fff;
  position: absolute;
  top: 0;
  right: 0;
  width: 100%;
  height: 100%;
}

.oa-toast > section > div > div {
  width: 100%;
  animation-name: preloader;
  animation-duration: 4s;
}

@keyframes preloader {
  from {
    width: 100%;
  }
  to {
    width: 0;
  }
}*/
</style>
