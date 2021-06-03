<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <button class="btn btnPrimary" @click="modalFirst = !modalFirst">
            Show firstmodal
          </button>
          <modals
            title="First title"
            v-show="modalFirst"
            @close="modalFirst = false"
          >
            <div slot="body">
              <p>Text Text Text Text Text Text Text</p>
              <button class="btn btnPrimary" @click="modalFirst = !modalFirst">
                Well done!
              </button>
            </div>
          </modals>
          <button
            class="btn btnPrimary"
            @click="modalSecond.show = !modalSecond.show"
          >
            Show modal with modal
          </button>
          <modals
            title="Modal with form"
            v-show="modalSecond.show"
            @close="modalSecond.show = false"
          >
            <div slot="body">
              <form @submit.prevent="submitSecondForm">
                <label>Name:</label>
                <input type="text" v-model="modalSecond.name" />
                <label>Email:</label>
                <input type="text" v-model="modalSecond.email" />
                <button class="btn btnPrimary">Submit!</button>
              </form>
            </div>
          </modals>

          <button
            class="btn btnPrimary"
            @click="modalValidate = !modalValidate"
          >
            Show modal with from + Validate
          </button>

          <modalValidate
            v-show="modalValidate"
            @close="modalValidate = false"
          />
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import modals from "@/components/UI/Modal.vue";
import ModalValidate from "@/components/ModalValidate.vue";
export default {
  components: {
    modals,
    ModalValidate,
  },
  data() {
    return {
      modalFirst: false,
      modalSecond: {
        show: false,
        name: "",
        email: "",
      },
      modalValidate: false,
    };
  },
  methods: {
    submitSecondForm() {
      console.log({
        name: this.modalSecond.name,
        email: this.modalSecond.email,
      });
      this.modalSecond.name = "";
      this.modalSecond.email = "";
      this.modalSecond.show = false;
    },
  },
};
</script>

