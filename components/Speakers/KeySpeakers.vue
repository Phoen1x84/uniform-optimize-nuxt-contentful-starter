<template>
  <Personalize :variations="variations" tracking-event-name="keySpeakerPersonalized">    
    <template v-slot:default="{ personalized, variations }" v-if="!mappedPersonalised">
      <div class="key-speaker">
        <section class="key-speaker__inner">
          <h2 class="key-speaker__title">{{ personalized ? fields.titleWhenPersonalised : fields.name }}</h2>
          <article class="key-speaker__card" v-for="(item, index) in variations" :key="index">
            <figure class="key-speaker__card-img" v-if="imageFormatProps(item).src">
              <img
                :src="imageFormatProps(item).src"
                :alt="imageFormatProps(item).alt"
                :width="imageFormatProps(item).width"
                :height="imageFormatProps(item).height"
                loading="lazy"
              />
            </figure>
            <div class="key-speaker__card-content">
              <h3 class="key-speaker__subtitle">{{item.fields.name}}</h3>
              <p class="key-speaker__desc">{{ item.fields.jobTitle}}</p>
            </div>
          </article>
        </section>
      </div>
    </template>
    <template v-slot:default="{ personalized, variations }" v-else>
      <div class="key-speaker">
        <section class="key-speaker__inner">
          <h2 class="key-speaker__title">{{ personalized ? fields.titleWhenPersonalised : fields.name }}</h2>
          <article class="key-speaker__card" v-for="(item, index) in variations" :key="index">
            <figure class="key-speaker__card-img" v-if="item.image.src">
              <img
                :src="item.image.src"
                :alt="item.image.alt"
                :width="item.image.width"
                :height="item.image.height"
                loading="lazy"
              />
            </figure>
            <div class="key-speaker__card-content">
              <h3 class="key-speaker__subtitle">{{item.name}}</h3>
              <p class="key-speaker__desc">{{ item.jobTitle}}</p>
            </div>
          </article>
        </section>
      </div>
    </template>
  </Personalize>
</template>
<script>
import { Personalize } from "@uniformdev/optimize-tracker-vue";
import { contentfulOptimizeListReader } from "@uniformdev/optimize-tracker-contentful";

export default {
  name: "key-speakers",
  components: {
    Personalize,
  },
  data() {
    return {
      mappedPersonalised: false
    }
  },
  props: {
    fields: {
      type: Object,
      default() {
        return {
          unfrmOptP13nList: {},
        };
      },
    },
    sys: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  computed: {
    variations() {
      if(!this.mappedPersonalised) {
        return contentfulOptimizeListReader(this.fields.unfrmOptP13nList);
      }
      return contentfulOptimizeListReader(this.fields.unfrmOptP13nList).map((item, index) => {
        return {
          id: index,
          jobTitle: item.fields.jobTitle,
          name: item.fields.name,
          image: {
            alt: item.fields.photo.fields.title,
            src: item.fields.photo.fields.file.url,
            ...item.fields.photo.fields.file.details.image
          }
        }
      });    
    },
  },
  mounted() {
    this.$uniform.optimize.trackBehavior(this.fields.unfrmOptIntentTag);    
  },
  methods: {
    imageFormatProps(context) {
      return {
        alt: context?.fields?.photo?.fields?.title,
        src: context?.fields?.photo?.fields?.file?.url,
        ...context?.fields?.photo?.fields?.file?.details?.image
      }
    }
  }
};
</script>
<style lang="scss">
.key-speaker {
  background: #fff;
  color: #2d3748;

  &__inner {
    max-width: 1280px;
    margin: 0 auto;
    padding: 2rem 0;
    width: 100%;
  }

  &__title {
    text-align: center;
    font-size: clamp(1rem, calc(1vw + 1rem), 3rem);
    font-weight: bold;
    max-width: 120ch;
  }

  &__card {
    display: flex;
    flex-wrap: wrap;
    padding: 1.5rem;
    margin: 0 1.5rem;
  }

  &__card-img {
  }

  &__card-content {
    padding: 1rem;
  }

  &__subtitle {
    font-weight: bold;
    font-size: clamp(1rem, calc(1vw + 1rem), 1.5rem);
  }

  &__desc {
    font-size: 1rem;
  }
}
</style>

