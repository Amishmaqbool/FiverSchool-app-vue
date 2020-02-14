<template>

<div class="container-fluid">
<div class="row">
 <div class="col-md-offset-4 col-md-4">
     <h4 class="myclass">UserName: {{username}}</h4>
 <h4 class="myclass">Email: {{email}}</h4>

   
    <input type="text" style="margin-top:20px; margin-bottom:20px;" v-model="search" placeholder="Search title.."/>
    <button @click="search='price'"> Sort by Price</button>
    <button @click="search='review'"> Sort by Review</button>
    <button @click="search='title'">sort alphabetically</button>
    
    <table class="table" style="color:white">
    <thead>
      <tr>
        <th>Topic</th>
        <th>Price</th>
        <th>Review</th>
        <th>Location</th>
        <th>Time</th>
      </tr>
    </thead>
    <tbody v-for="post in filteredList">
      <tr>
        <td> {{ post.title }}</td>
        <td> {{ post.price }}</td>
        <td> {{ post.review }}</td>
        <td> {{ post.Location }}</td>
        <td> {{ post.time }}</td>
      </tr>
      
    </tbody>
  </table>
    <div class="form-group">
      <label style="color:white">Review:</label>
      <select class="form-control" name="user_review" v-model="user_review">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
      <option value="4">4</option>
      <option value="5">5</option>
      </select>
      <button style="margin-top:10px;" class="form-control" @click="save_review()"> Saved</button>
    </div>
     </div>
        <div class="col-md-4" v-if="type=='service_provdier'">
        <form @submit.prevent="add_activity" novalidate>
          <h2 style="color:white; margin-top: 23%;">Add</h2>
          <div class="input-container">
            <i class="fa fa-user icon"></i>
            <input
              class="input-field"
              type="text"
              v-model="add_topic"
              v-validate="'required'"
              placeholder="Class/Activity"
              name="add_topic"
            />
          </div>
           <p style="color:red" v-if=" errors.first('add_topic')">Topic is Required.</p>
          <div class="input-container">
            <i class="fa fa-envelope icon"></i>
            <input
              class="input-field"
              type="text"
              v-model="add_price"
               v-validate="'required'"
              placeholder="Price"
              name="add_price"
            />
          </div>
           <p style="color:red" v-if=" errors.first('add_price')">Price is Required</p>
          <div class="input-container">
            <i class="fa fa-key icon"></i>
            <input
              class="input-field"
              type="text"
              v-model="add_review"
               v-validate="'required'"
              placeholder="Review"
              name="add_review"
            />
          </div>
         <p style="color:red" v-if=" errors.first('add_review')">Review is Required.</p>
          <button type="submit" class="btn">Add</button>
        </form>
      </div>

</div>
</div>

</template>

<script>
class Post {
  constructor(title, price, review, Location, time) {
    this.title = title;
    this.price = price;
    this.review = review;
    this.Location = Location;
    this.time = time;
  }
}
export default{
 name:'dasboard',
 data(){
   return{
         username:'',
         email:'',
         user_review:'1',
         add_review:'',
         add_price:'',
         add_topic:'',
         search:'',
         type:'',
        postList : [
      new Post(
        'Math ', 
        '7000', 
        'Good', 
        'london',
        '4:00am',
      ),
      new Post(
        'English', 
        '6000', 
        'Normal',
        'paris',
        '9:00am',
      ),
      new Post(
        'Sport Club', 
        '1500', 
        'Its awesome', 
        'France',
        '4:00pm',
      ),
      new Post(
        'Test Club', 
        '2000', 
        'Great',
        'barlin',
        '4:00pm',
      ),
      new Post(
        'Cricket', 
        '1000', 
        'love to play', 
        'london',
        '5:00pm',
      ),
      new Post(
        'Football', 
        '700', 
        'Nice',
        'paris',
        '6:00pm',
      ),
      
        ]
   }
 },
    computed: {
    filteredList() {
      if(this.search=='price'){
      return this.postList.sort(function(a, b) {
        return a.price - b.price;
      });
      }
      else if(this.search=='review'){
      return this.postList.sort(function(a, b) {
           if (a.review < b.review){
                 return -1;}
          if (a.review > b.review){
                 return 1;
           }
          return 0;
      });
      }
      else if(this.search=='title'){
      return this.postList.sort(function(a, b) {
           if (a.title < b.title){
                 return -1;}
          if (a.title > b.title){
                 return 1;
           }
          return 0;
      });
      }
      else{
       return this.postList.filter(post => {
        return post.title.toLowerCase().includes(this.search.toLowerCase())
      })
      }

    }
  },
 mounted(){
   var user = JSON.parse(localStorage.getItem("user"));
    if(user){

      this.username = user.user_name;
      this.email = user.email;
      this.type =  user.type;
    }
 },
 methods:{
    save_review(){
      var u_review = localStorage.getItem("review");
      if(!u_review){
       localStorage.setItem("review",this.user_review);
       alert("Review Saved");
      }
      else{
        alert("Cannot update your review");
      }
   },
   add_activity(){
     this.$validator.validateAll().then((result) => {
         if(result){
          var new_record= {
            'title':this.add_topic,
            'price':this.add_price,
             'review':this.add_review 
          };

          this.postList.push(new_record);
           
     alert("Added");
         }
     });
   }

 }
}
</script>
<style>
.myclass{
    color:white;
}
body {
  font-family: Arial, Helvetica, sans-serif;
}
* {
  box-sizing: border-box;
}

.input-container {
  display: -ms-flexbox; /* IE10 */
  display: flex;
  width: 100%;
  margin-bottom: 15px;
}

.icon {
  padding: 10px;
  background: dodgerblue;
  color: white;
  min-width: 50px;
  text-align: center;
}

.input-field {
  width: 100%;
  padding: 10px;
  outline: none;
}

.input-field:focus {
  border: 2px solid dodgerblue;
}

/* Set a style for the submit button */
.btn {
  background-color: dodgerblue;
  color: white;
  padding: 15px 20px;
  border: none;
  cursor: pointer;
  width: 100%;
  opacity: 0.9;
}

.btn:hover {
  opacity: 1;
}
</style>