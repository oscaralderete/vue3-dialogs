# vue3-dialogs
Web app dialog components: modal + toast developed for Vue 3

(Live sample here: https://experiments.oscaralderete.com/vue3-dialogs/)

Finally I got some time to play with the Vue 3 CLI. Vue 3 looks great and I just scratch the surface, this time I decided to develop a pair of 'always used' components: a modal dialog and a toast. These elements need interact from parent (to send data) and the component itself (to run inner methods/events).

If you use the Vue CLI basically you can't feel the difference between v2 and v3, because the CLI provides you the blueprint but if you use develop SSR (server side rendering apps) you need to face a little new way to instantiate your app, the old straight way to instantiate your app:

```bash
const app = new Vue({ definition here... });
console.log(app.someAttribute);
app.someMethod();
```

changed to:

```bash
const app = Vue.createApp({ definition here... });
```

then mount it:

```bash
const mountedApp = app.mount('#vue-app');
```

so, if externally you need access to some Vue instance property or method you must use 'mountedApp' not 'app':

```bash
console.log(mountedApp.someAttribute);
mountedApp.someMethod();
```

Back to my components, check the samples please, but in short you need to call dialog and modal using "$refs" then using component method, in the case of toast component:

```bash
this.$refs.toastRefName.text({
  message: "This is the toast message!", //required
  type: "success", //optional, possible values: 'success', 'error' and 'warning'
  callback: this.myToastCallback //optional, runs when close or timeout
})
```
in the other case, you need to invoque the .show() method:

```bash
this.$refs.modalRefName.show({
  title: "ALERT", //optional, the dialog title
  message: "Your awesome modal message here!", //the only required property
  buttons: { //optional
    ok: { //optional
      label: 'OKI', //optional, default 'OK'
      callback: () => { //optional, you can call some predefined method or anonymous funtions
        console.log("This callback will run after click on OK button..");
      },
    },
    cancel: { //optional
      label: 'DISAGREE', //optional, default 'CANCEL'
      callback: this.predefinedCancelCallback //optional
    }
  },
})
```

Vue 3 is cool and now it's mature enough to use in your daily work, no matter if you develop projects using the CLI or coding SSR apps, try it, it's a 5-star!

You can play these dialogs here: https://experiments.oscaralderete.com/vue3-dialogs/
