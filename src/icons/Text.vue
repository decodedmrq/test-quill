<template>
  <svg version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px"
       :width="width"
       :height="height"
       y="0px"
       :aria-labelledby="iconName"
       viewBox="0 0 405.333 405.333">
    <g>
      <g>
        <g>
          <polygon points="213.333,149.333 213.333,213.333 277.333,213.333 277.333,362.667 341.333,362.667 341.333,213.333
				405.333,213.333 405.333,149.333"/>
          <polygon points="277.333,42.667 0,42.667 0,106.667 106.667,106.667 106.667,362.667 170.667,362.667 170.667,106.667
				277.333,106.667"/>
        </g>
      </g>
    </g>
  </svg>
</template>

<script>
export default {
  props: {
    iconName: {
      type: String,
      default: 'box'
    },
    width: {
      type: [Number, String],
      default: 18
    },
    height: {
      type: [Number, String],
      default: 18
    },
    iconColor: {
      type: String,
      default: 'currentColor'
    }
  }
}

</script>
