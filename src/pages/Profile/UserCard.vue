<template>
  <card type="user">
    <p class="card-text">
    </p>
    <div class="author">
      <a href="#">
        <img class="avatar" :src="user.image" alt="...">
        <h5 class="title">{{user.fullName}}</h5>
      </a>
      <p class="description">
        {{user.title}}
      </p>
    </div>
    <p></p>
    <div style="text-align:center">
      <base-button slot="footer" type="button" @click="$router.push('edit/'+user.id)" fill>Edit</base-button>

    </div>
  </card>
</template>
<script>
  export default {
    props: {
      user: {
        type: Object,
        default: () => {
          return {};
        }
      }
    }
  }
</script>
<style>
</style>
