<script>
export default {
  data() {
    return {
      error: '',
      user: null
    };
  },
  mounted() {
    // Check for token in localStorage
    const token = localStorage.getItem('token');
    if (!token) {
      this.$router.push('/login');
      return;
    }

    fetch('https://dummyjson.com/auth/me', {
      method: 'GET',
      headers: {
        'Authorization': 'Bearer ' + token
      }
    })
      .then(response => {
        if (!response.ok) {
          throw new Error('Authentication Problem');
        }
        return response.json();
      })
      .then(data => {
        this.user = data;
      })
      .catch(error => {
        this.error = error.message;
      })
  }
};
</script>

<template>
    <div>
      <div class="user-layout">
        <h1 class="error" v-if="error">{{ error }}</h1>
        <div class="user-content" v-if="user">
            <h2>My profile</h2>
            <div class="info">
                <div>
                    <p>Username:{{ user.username }}</p>
                    <p>Name:{{ user.firstName }}</p>
                    <p>Lastnane:{{ user.lastName }}</p>
                    <p>Gender:{{ user.gender }}</p>
                    <p>Email:{{ user.email }}</p>
                </div>
                    <img :src="user.image" alt="pfp">
            </div>

        </div>
      </div>
    </div>
  </template>

<style scoped>
    .user-layout{
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: center;
        height: 500px;
    }
    .error{
        color: rgb(33, 36, 61);
        font-family: Heebo;
        font-size: 44px;
        font-weight: 700;
        line-height: 60px;
        letter-spacing: 0px;
        
        margin-left: auto;
        margin-right: auto;

        width: fit-content;

        align-self: center;

        margin-top: 70px;
    }
    .user-content{
        height: 500px;
        width: 600px;

        margin-left: auto;
        margin-right: auto;
    }
    .info{
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }

    img{
        height: 243px;
        width: 243px;
        border-radius: 50%;
    }

    h2{
        color: rgb(33, 36, 61);
        font-family: Heebo;
        font-size: 44px;
        font-weight: 700;
        line-height: 60px;
        letter-spacing: 0px;
        text-align: left;
    }

    p{
        border-radius: 10px;
        box-shadow: 0px 2.75px 9px 0px rgba(0, 0, 0, 0.19),0px 0.25px 3px 0px rgba(0, 0, 0, 0.04);
        width:fit-content;
        text-align: left;

        padding: 10px 30px 10px 10px;

        font-weight: 600;
    }
</style>
  
