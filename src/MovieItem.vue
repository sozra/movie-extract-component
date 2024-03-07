<script setup>
// Start your logic here
import { computed, reactive, ref } from "vue";
/*
These are Icons that you can use, of course you can use other ones if you prefer.
*/
import { StarIcon, TrashIcon, PencilIcon } from "@heroicons/vue/24/solid";


const props = defineProps({
  movie: {
    type: Object,
    required: true,
  },
}
)

const emits = defineEmits(["update-rating", "remove-movie", "edit-movie"]);


function updateRating(id, rating) {
  // movies.value[movieIndex].rating = rating;
  emits("update-rating", id, rating);
}
function removeMovie(id) {
  // movies.value = movies.value.filter((movie, index) => index !== movieIndex);
  emits("remove-movie", id);
}
function editMovie(id) {
  // const movie = movies.value[movieIndex];
  emits("edit-movie", id);
}

</script>

<template>
  <!-- Start your template here -->
  <div
        class="movie-item group"

      >
        <div class="movie-item-image-wrapper">
          <div class="movie-item-star-wrapper">
            <StarIcon
              id="rating"
              class="movie-item-star-rating-icon"
              :class="[props.movie.rating ? 'text-yellow-500' : 'text-gray-500']"
            />
            <div class="movie-item-star-content-wrapper">
              <span
                v-if="props.movie.rating"
                id="rating-stars"
                class="movie-item-star-content-rating-rated"
              >
                {{ props.movie.rating }}
              </span>
              <span v-else class="movie-item-star-content-rating-not-rated">
                -
              </span>
            </div>
          </div>
          <img :src="props.movie.image" class="movie-item-image" alt="" />
          <!-- <img src="https://placehold.co/600x900/cyan/white" class="movie-item-image" alt="" /> -->
        </div>

        <div class="movie-item-content-wrapper">
          <div class="movie-item-title-wrapper">
            <h3 class="movie-item-title">{{ props.movie.name }}</h3>
            <div class="movie-item-genres-wrapper">
              <span
                v-for="genre in props.movie.genres"
                :key="`${props.movie.id}-${genre}`"
                class="movie-item-genre-tag"
                >{{ genre }}</span
              >
            </div>
          </div>
          <div class="movie-item-description-wrapper">
            <p class="movie-item-description">{{ props.movie.description }}</p>
          </div>
          <div class="movie-item-rating-wrapper">
            <span class="movie-item-rating-text">
              Rating: ({{ props.movie.rating }}/5)
            </span>
            <div class="movie-item-star-icon-wrapper">
              <button
                v-for="star in 5"
                :key="star"
                class="movie-item-star-icon-button"
                :class="[
                  star <= props.movie.rating ? 'text-yellow-500' : 'text-gray-500',
                ]"
                :disabled="star === props.movie.rating"
                @click="updateRating(props.movie.id, star)"
              >
                <StarIcon class="movie-item-star-icon" />
              </button>
            </div>

            <div class="movie-item-actions-list-wrapper">
              <button
                class="movie-item-action-edit-button"
                @click="editMovie(props.movie.id)"
              >
                <PencilIcon class="w-4 h-4" />
              </button>
              <button
                class="movie-item-action-remove-button"
                @click="removeMovie(props.movie.id)"
              >
                <TrashIcon class="w-4 h-4" />
              </button>
            </div>
          </div>
        </div>
      </div>
</template>
