<template>
  <card type="user">
    <p class="card-text">
    </p>
    <div class="author">
      <a href="#">
        <img class="avatar" :src=user.image alt="...">
        <h5 class="title">{{user.fullName}}</h5>
      </a>
      <p class="description">
        {{user.title}}
      </p>
    </div>
    <p></p>
    <base-button slot="footer" type="button" fill>Edit</base-button>
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
