<template>
  <div class="mb-4">
    <div class="card border-0">
      <div class='img_ratio'>
        <img :src="src" class="image_scale img_frame" />
      </div>
      <div class="card-body">
        <h4 class="card-title"><slot></slot></h4>
        <span class="custome_text"
          ><a
            class="btn btn-custom"
            data-toggle="collapse"
            :href="'#' + id"
            role="button"
            aria-expanded="false"
            :aria-controls="'#' + id"
          >
            Summary
          </a></span
        >
        <br /><br />
        <div class="collapse" :id="id">
          <div class="card card-body border-0">
            <slot name="des"></slot>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Movie",
  props: ["src", "id"],
};
</script>
