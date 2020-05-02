<template>
  <div>
    <div class="container-fluid p-0 m-0">
      <h2 class="text-center mt-3" style="color:#708160;">
        <strong style="color:#dd7631;">Yandex</strong> Api
      </h2>
      <div class="colorEffect"></div>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-md-12 textArea shadow">
          <div class="col-md-12">
            <button class="btn btn-info mt-3">
              <i class="fab fa-acquisitions-incorporated"></i> Metin
            </button>
            <hr />
          </div>
          <div class="col-md-12">
            <button @click="changeLanguage" class="btn btn-light">
              <i class="fas fa-language"></i>
              {{languageTr}}
            </button>
            <button class="btn btn-light float-right">
              <i class="fas fa-globe-americas"></i>
              {{languageEng}}
            </button>
          </div>
          <div class="col-md-12 mb-5 mt-3">
            <div class="form-group d-flex justify-content-center">
              <textarea
                @keyup.enter="translate"
                class="textarea form-control mr-3"
                v-model="text"
                placeholder="Translate..."
                rows="8"
              ></textarea>
              <textarea
                readonly
                style="background-color:#fff;"
                class="textarea form-control"
                placeholder="Translate..."
                rows="8"
                v-model="translateText"
              ></textarea>
            </div>
          </div>
        </div>
        <div class="col-md-12 mt-3">
          <h4 style="color: #dd7631;">
            <i class="far fa-clone"></i> Geçmiş
          </h4>
          <hr />
          <div class="col-md-6">
            <p v-for="(translatedText,index) in pastTranslate" :key="index">
              <i class="fab fa-tumblr"></i>
              {{translatedText.text}} --> {{translatedText.translated}}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      text: "",
      translateText: "",
      pastTranslate: [],
      languageStatus: false,
      language: "tr-en",
      languageTr: "Türkçe",
      languageEng: "English"
    };
  },

  methods: {
    translate() {
      axios
        .post(
          "https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20200501T044207Z.9e166439d71823ea.14569cfd4e18a4ff98cf6063d6e26b43832e5ecf&text=" +
            this.text +
            "&lang=" +
            this.language +
            ""
        )
        .then(response => {
          this.translateText = response.data.text[0];
          let past = {
            text: this.text,
            translated: this.translateText
          };
          this.pastTranslate.push(past);
        });
    },
    changeLanguage() {
      if (this.languageStatus === false) {
        this.languageStatus = true;
        this.language = "en-tr";
        this.languageTr = "English";
        this.languageEng="Türkçe"
      } else if (this.languageStatus === true) {
        this.languageStatus = false;
        this.language = "tr-en";
        this.languageTr = "Türkçe",
        this.languageEng = "English"
      }
    }
  }
};
</script>

<style>
.colorEffect {
  background-color: #f2f2f2;
  height: 250px;
  width: 100%;
  border-top: 1px solid #d2d2d2;
  border-bottom: 1px solid #d2d2d2;
  position: absolute;
  top: 65px;
  left: 0;
}

.textArea {
  height: auto;
  background-color: #fff;
  margin-top: 15px;
  border-radius: 30px;
}

textarea {
  resize: none;
}

.textarea {
  border: 0;
  overflow: auto;
  border-right: 1px solid #d2d2d2;
}

.textarea:focus {
  box-shadow: inset 0 0px 0 #ddd;
}
</style>
