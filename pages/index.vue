<template>
  <div class="flex justify-center items-center h-[100vh]">
    <button @click="prev">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="size-6"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M6.75 15.75 3 12m0 0 3.75-3.75M3 12h18"
        />
      </svg>
    </button>

   
    <div
      class="carousel flex justify-center items-center w-[160vh] mr-5 ml-5 overflow-hidden max-sm:w-[30vh] 2xl:w-[90%]"
      ref="carousel"
    >
      <div
        class="inner w-[35%] bg-green-70 transition-transform duration-300 whitespace-nowrap max-sm:w-[90%] sm:w-[25%] lg:w-[30%] lg:ml-10 xl:w-[25%] 2xl:w-[40%]"
        ref="inner"
        :style="innerStyles"
      >
        <div class=" inline-flex" v-for="card in cards" :key="card.id">
          <div
            class="card flex flex-col flex-wrap items-center justify-between mr-10 self-stretch w-[45vh] h-[59vh] pb-5 pt-10 px-10 border border-solid border-stone-900 max-sm:w-[26vh] max-sm:px-0  max-sm:h-[50vh] max-sm:mr-12"
          >
            <div class="w-full self-center break-words whitespace-normal mb-10 max-sm:w-[24vh] max-sm:mx-2 max-sm:text-xs sm:text-xs">
              <h1 class="mb-2 bold">{{ card.title }}</h1>
              <h3>
                {{ card.subTitle }}
              </h3>
            </div>

            <img class="w-[20vh] max-sm:w-[10vh] max-sm:mb-10  sm:w-[15vh]" :src="card.image" alt="#" />
            <button
              class="bg-violet-500  text-white text-md px-11 py-2 max-sm:px-5"
            >
              Discover
            </button>
          </div>
        </div>
      </div>
    </div>  
    <button @click="next">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="size-6"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M17.25 8.25 21 12m0 0-3.75 3.75M21 12H3"
        />
      </svg>
    </button>



<!-- Bullets -->
<ul class="absolute bottom-[15%] flex justify-center ">
  <li v-for="(card, index) in cards" :key="card.id"  :class="{ 'bg-gray-700  , rotate-45': currentIndex === index, 'bg-gray-300 ': currentIndex !== index  ,}" class="w-2  h-2 duration-300 mx-1 rounded-sm select-none "></li>
</ul>


  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const carousel = ref(null);
const inner = ref(null);
const innerStyles = ref({});
let step = "";
let transitioning = false;
let currentIndex = ref(0);


//carousel slide
// add a child to add another slide //
const cards = [
  {
    id: 1,
    image: "/image/Hero.png",
    title: "EmpowHER",
    subTitle:
      "Our mission is to empower women facing employment challenges,helping them secure jobs and achieve independence.",
  },
  {
    id: 2,
    image: "/image/Journal.png",
    title: "JournHEALism",
    subTitle:
      "The newspaper of our center, written by the same women we assist day after day. Heal through writing.",
  },
  {
    id: 3,
    image: "/image/Lighthouse.png",
    title: "SEAfer",
    subTitle:
      "Sail with us and forget all your worries as you navigate through the waves. Feel safer at sea.",
  },
  {
    id: 3,
    image: "/image/Lighthouse.png",
    title: "SEAfer",
    subTitle:
      "Sail with us and forget all your worries as you navigate through the waves. Feel safer at sea.",
  },
  {
    id: 3,
    image: "/image/Lighthouse.png",
    title: "SEAfer",
    subTitle:
      "Sail with us and forget all your worries as you navigate through the waves. Feel safer at sea.",
  },
  
];


onMounted(() => {
  setStep();
  resetTranslate();
});

const setStep = () => {
  const innerWidth = inner.value.scrollWidth;
  const totalCards = cards.length;
  step = `${innerWidth / totalCards}px`;
};

const next = () => {
  if (transitioning) return;

  transitioning = true;

  if(currentIndex.value < cards.length - 1){

    currentIndex.value++

  }else{
    currentIndex.value = 0
  }

  moveLeft();

  afterTransition(() => {
    const card = cards.shift();
    cards.push(card);
    resetTranslate();
    transitioning = false;
  });
};

const prev = () => {
  if (transitioning) return;

  transitioning = true;
  if(currentIndex.value >0){

    currentIndex.value--
  }else{
    currentIndex.value = cards.length-1
  }
  moveRight();

  afterTransition(() => {
    const card = cards.pop();
    cards.unshift(card);
    resetTranslate();
    transitioning = false;
  });
};

const moveLeft = () => {
  innerStyles.value = {
    transform: `translateX(-${step}) translateX(-${step})`,
  };
};

const moveRight = () => {
  innerStyles.value = {
    transform: `translateX(${step}) translateX(-${step})`,
  };
};

const afterTransition = (callback) => {
  const listener = () => {
    callback();
    inner.value.removeEventListener("transitionend", listener);
  };
  inner.value.addEventListener("transitionend", listener);
};

const resetTranslate = () => {
  innerStyles.value = {
    transition: "none",
    transform: `translateX(-${step})`,
  };
};


</script>
