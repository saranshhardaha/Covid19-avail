<template>
  <div id="Home" class="flex-allcenter">
    <div class="toast-warning">
      Do NOT make advanced payments unless you are 100% sure about their
      authenticity.
      <hr />
      Check for replies under the tweets.
    </div>

    <section>
      <h4>Search for COVID19 resources in your city 🏙 using Social Media.</h4>

      <div class="search-container">
        <h1>Search</h1>
        <p>Enter <b>city</b> name:</p>
        <input
          type="text"
          @keyup.enter="search"
          name="cityname"
          id="city-name"
        />

        <div class="checkBoxs">
          <div v-for="(item, key, index) in checkBox" :key="index">
            <input
              class="styled-checkbox"
              :id="`checkbox-${index}`"
              type="checkbox"
              v-model="item.checked"
              name="alsoSearchfor"
            />
            <label :for="`checkbox-${index}`">{{ key }}</label>
          </div>
        </div>
        <!-- <div class="otherInput">
        <label for="other">Other: </label>
        <input type="text" name="other" id="other" />
      </div> -->
        <button class="flex-center" v-on:click="search">Search</button>
      </div>

      <hr />

      <div class="extraOptions">
        <h4>Tweets should NOT have these words(needed,required):</h4>
        <div class="checkBoxs ochk">
          <div v-for="(item, key, index) in excludeKeywords" :key="index">
            <input
              :id="`exclude-${index}`"
              type="checkbox"
              v-model="item.checked"
              name="alsoSearchfor"
            />
            <label :for="`exclude-${index}`">{{ key }}</label>
          </div>
        </div>
        <!-- <div class="otherInput">
        <label for="other">Other: </label>
        <input type="text" name="other" id="other" />
      </div> -->

        <hr />

        <div class="checkBoxs ochk">
          <div v-for="(item, key, index) in includeKeywords" :key="index">
            <input
              :id="`include-${index}`"
              type="checkbox"
              v-model="item.checked"
              name="alsoSearchfor"
            />
            <label :for="`include-${index}`">{{ item.title }}</label>
          </div>
        </div>
        <p>* Uncheck "show verified" for <b>smaller cities</b></p>
      </div>
    </section>
  </div>
</template>

<script>
import { ref } from "vue";
// @ is an alias to /src

export default {
  name: "Home",
  setup() {
    let REDIRECT_URL = ref("");

    let checkBox = {
      Beds: {
        keywords: ["bed", "beds"],
        checked: true,
      },
      ICU: {
        keywords: ["icu"],
        checked: true,
      },
      Oxygen: {
        keywords: ["oxygen"],
        checked: true,
      },
      Ventilator: {
        keywords: ["ventilator", "ventilators"],
        checked: true,
      },
      Tests: {
        keywords: ["test", "tests", "testing"],
        checked: false,
      },
      Fabiflu: {
        keywords: ["fabiflu"],
        checked: false,
      },
      Remdesivir: {
        keywords: ["remdesivir"],
        checked: false,
      },
      Favipiravir: {
        keywords: ["favipiravir"],
        checked: false,
      },
      Tocilizumab: {
        keywords: ["tocilizumab"],
        checked: false,
      },
      Plasma: {
        keywords: ["plasma"],
        checked: false,
      },
      Food: {
        keywords: ["tiffin", "food"],
        checked: false,
      },
    };
    let includeKeywords = {
      verified: {
        title: "Show verified tweets only *",
        keywords: ["verified"],
        checked: true,
      },
      unverified: {
        title: "Exclude unverified tweets",
        keywords: ["not-verified", "unverified"],
        checked: true,
      },
    };
    let excludeKeywords = {
      needed: {
        keywords: ["needed", "need", "needs"],
        checked: true,
      },
      required: {
        keywords: [
          "required",
          "require",
          "requires",
          "requirement",
          "requirements",
        ],
        checked: true,
      },
    };

    function alsoKeywords() {
      let searchKeywords = [];
      let keye = [];

      if (includeKeywords.unverified.checked) {
        let keywrd = includeKeywords.unverified.keywords;
        keywrd.forEach((element) => {
          keye.push(element);
        });
      }

      // Keywords for requirements/Resources
      Object.values(checkBox).map((ele) => {
        if (ele.checked) {
          searchKeywords.push(`${ele.keywords.join("+OR+")}`);
        }
      });

      // Excluded Keywords (needed/required)
      Object.values(excludeKeywords).map((ele) => {
        if (ele.checked) {
          ele.keywords.forEach((element) => {
            keye.push(element);
          });
        }
      });
      // Mapping keywords in proper format for url for eg: ` -"KEYWORD" `
      keye = keye.map((k) => `-"${k}"`).join("+");

      return `(${searchKeywords.join("+OR+")})+${keye}&f=live`;
    }

    function search() {
      let cityName = document.getElementById("city-name").value;

      if (cityName.length !== 0) {
        let BASE_URL = "https://twitter.com/search?q=";
        let keywords = alsoKeywords();
        let isVerified = ref("");
        let query = ref("");

        // Include Verifed keyword
        if (includeKeywords.verified.checked) {
          isVerified.value = includeKeywords.verified.keywords;
          query.value = `${isVerified.value}+${cityName}+${keywords}`;
        } else {
          query.value = `${cityName}+${keywords}`;
        }

        REDIRECT_URL.value = `${BASE_URL}${query.value}`;

        // Open url in different tab
        window.open(REDIRECT_URL.value, "_blank");
      } else {
        //If cityName Input is empty
        alert("Enter  city name");
      }
    }

    return {
      checkBox,
      search,
      excludeKeywords,
      includeKeywords,
    };
  },
};
</script>

<style lang="scss" scoped>
* {
  text-align: left;
}
#Home {
  flex-direction: column;
}

hr {
  height: 1px;
  margin: 22px 0;
  background: rgba(0, 0, 0, 0.2);
  outline: none;
  border: none;
}
.toast-warning {
  padding: 12px 18px;
  width: 100%;
  text-align: center;
  font-size: 15px;
  font-weight: bold;
  background: rgba($primary, 0.5);
  font-family: "Roboto Mono", monospace;
  hr {
    background: rgba(0, 0, 0, 0.12);
    margin: 10px 0;
  }
}
section {
  margin: 12px;
  min-height: calc(100vh - 250px);
  max-width: 720px;
  padding: 50px;
  h1,
  h4 {
    font-family: "Roboto Mono", monospace;
    font-weight: bold;
  }
  h1 {
    width: max-content;
  }
  h1::after {
    content: " ";
    display: block;
    height: 4px;
    width: 20%;
    background: $primary;
  }
  p {
    margin: 4px 0;
  }
  input[type="text"] {
    width: 100%;
    outline: none;
    border: solid 2px rgba(#3dd28d, 0.8);
    background: rgba(#fff, 0.8);
    font-family: sans-serif;
    font-size: 15px;
    border-radius: 4px;
    padding: 10px 12px;
  }
  button {
    display: flex;
    overflow: hidden;
    background: #3dd28d;
    margin: 10px 0;
    padding: 10px 28px;

    cursor: pointer;
    transition: all 150ms linear;
    text-align: center;
    text-decoration: none !important;
    text-transform: capitalize;

    color: #000000;
    font-family: "Roboto Mono", monospace;
    border: 0 none;
    border-radius: 36px;

    line-height: 1.3;
    box-shadow: 2px 5px 10px #e4e4e4;

    &:hover {
      opacity: 0.75;
    }

    &:active {
      transition: all 150ms linear;
      opacity: 0.75;
    }
  }
}
.otherInput input {
  padding: 8px 12px !important;
  font-size: 14px !important;
  font-weight: normal !important;
}
.checkBoxs {
  margin: 12px 0;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 12px;

  div {
    position: relative;
    display: inline-block;

    input[type="checkbox"] {
      position: absolute; // take it out of document flow
      opacity: 0; // hide it

      & + label {
        position: relative;
        cursor: pointer;
        padding: 0;
      }

      // Box.
      & + label:before {
        content: "";
        margin-right: 10px;
        display: inline-block;
        vertical-align: text-top;
        width: 20px;
        height: 20px;
        background: rgba($primary, 0.3);
        border-radius: 2px;
      }

      // Box hover
      &:hover + label:before {
        background: rgba($primary, 0.7);
      }

      // Box focus
      &:focus + label:before {
        box-shadow: 0 0 0 3px rgba(0, 0, 0, 0.12);
      }

      // Box checked
      &:checked + label:before {
        background: rgba($primary, 1);
      }

      // Disabled state label.
      &:disabled + label {
        color: #b8b8b8;
        cursor: auto;
      }

      // Disabled box.
      &:disabled + label:before {
        box-shadow: none;
        background: #ddd;
      }

      // Checkmark. Could be replaced with an image
      &:checked + label:after {
        content: "";
        position: absolute;
        left: 5px;
        top: 9px;
        background: white;
        width: 2px;
        height: 2px;
        box-shadow: 2px 0 0 white, 4px 0 0 white, 4px -2px 0 white,
          4px -4px 0 white, 4px -6px 0 white, 4px -8px 0 white;
        transform: rotate(45deg);
      }
    }
  }
}
.ochk {
  grid-template-columns: repeat(2, 1fr) !important;
}
@media only screen and (max-width: 600px) {
  section {
    padding: 20px;
    input[type="text"] {
      width: 100%;
    }
  }
  .toast-warning {
    font-size: 14px;
  }
  .checkBoxs {
    grid-template-columns: repeat(2, 1fr);
    div {
      label {
        font-size: 14px;
      }
      input {
        & + label:before {
          height: 15px;
          width: 15px;
        }
        &:checked + label:after {
          left: 3px;
          top: 7px;
        }
      }
    }
  }
}
</style>
