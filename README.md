
## Github Finder 

### Step 1: Create a New Directory and Initialize a New Git Repository

```
mkdir github-finder
cd github-finder
git init
```

### Step 2: Create index.html File

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Github Finder</title>
    <link rel="stylesheet" href="style.css">
  
</head>
<body>
    <div class="container">
        <header class="header d-flex between center">
            <h2 class="logo">I'm Your Github Finder</h2>
        </header>
        <form class="search-form d-flex between center">
            <i class="far fa-fw fa-search"></i>
            <input type="text" placeholder="Kindly Enter UserName" class="keyword">
            <button class="btn" type="submit">Search</button>
        </form>

        <div class="user-card">
            <div class="loader">This Section is for Display Details...</div>
        </div>
    </div>
    <script src="index.js"></script>
</body>
</html>
```
### Step 3: Create style.css File

```
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-weight: 900;
    font-family: SamsungOne, arial, sans-serif;;
 
}

body{
    display: grid;
    place-items: center;
    height: 100vh;
}

.header{
    margin-bottom: 20px;
}

.header .logo{
    font-size: 2rem;
    font-weight:bold
}

.search-form{
    gap: 10px;
    padding: 12px 14px;
    border-radius: 10px;
    background-color: #ededed;
    margin-bottom: 20px;
}

.search-form .fa-search{
    font-size:1.3rem;
}

.search-form .keyword{
    width: 100%;
    padding: 8px;
    background-color: transparent;
}

.search-form .btn{
    padding: 8px 12px;
    border-radius: 6px;
    background-color: black;
    color: white;
    cursor: pointer;
    transition: .4s all ease;
}

.search-form .btn:hover{
    transform: scale(0.95);
}

.user-card{
    background-color: #ededed;
    border-radius: 10px;
    padding: 30px;
}

.container{
    width: 80%;
}

.d-flex{
    display: flex;
}

.d-flex.between{
    justify-content: space-between;
}

.d-flex.center{
    align-items: center;
}

img{
    width: 100%;
    object-fit: cover;
}

.header .dark-toggle{
    text-transform: uppercase;
    letter-spacing: 5px;
}

.user-card .head{
    margin-bottom: 20px;
}

.user-card .had .sub{
    flex-direction: column;
    -webkit-box-direction: normal;
}

.user-card .head .photo{
    width: 80px;
    border-radius: 50%;
    height: 80px;
    margin-right: 20px;
}

.user-card .head .name{
    font-size: 1.3rem;
    margin-bottom: 5px;
}

.user-card .head .joined{
    margin-top: 8px;
    font-size: .9rem;
}

.user-card .details .card{
    margin-top: 18px;
    padding: 22px;
    background-color: white;
    margin-bottom: 20px;
    border-radius: 8px;
    font-size: .9rem;
}

.user-card .details .links{
    flex-direction: row;
    -webkit-box-direction: normal;
}

.user-card .details .links li{
    margin-bottom: 12px;
}

.mb-5{
    margin-bottom: 5px;
}

.fw-bold{
    font-weight: bold;
}

.w-100{
    width: 100%;
}
```

### Step 4: Create index.js File

```
// https://api.github.com/users/

let searchForm = document.querySelector(".search-form");
let userCardConatiner = document.querySelector(".user-card");
let loader = document.querySelector(".loader");
let getuser = async () => {
  let keyword = document.querySelector(".keyword");
  if (keyword.value.length <= 0) {
    userCardConatiner.innerHTML = loader.innerHTML;
  } else {
    loader.innerHTML = `<i class="fas fa-spin fa-spinner"></i>`;
    let response = await fetch(`https://api.github.com/users/${keyword.value}`);
    let result = await response.json();
    let data = await result;
    console.log(data);
    showuser(data);
    loader.innerHTML = 'Please enter your github username.'
  }
};
let showuser = (data)=>{
    if(data.message === 'Not Found'){
        loader.innerHTML = 'Uset not found';
        userCardConatiner.innerHTML = loader.innerHTML;
    }else{
        let userCradHtml = `<div class="head d-flex center">
        <img src="${data.avatar_url}" alt="${data.name}" class="photo">
        <div class="d-flex between w-100 sub">
            <div>
                <h1 class="name fw-bold">${(data.name) ? data.name : '-'}</h1>
                <a href="${data.html_url}" class="username" target="_blank">@${data.login}</a>
            </div>
            <p class="joined">Joined ${new Date(data.created_at).toLocaleDateString('en-US')}</p>
        </div>
    </div>
    <div class="details">
        <p class="bio">${(data.bio) ? data.bio : '-'}</p>
        <ul class="card d-flex between center">
            <li>
                <h6 class="mb-5">Repos</h6>
                <span class="fw-bold">${data.public_repos}</span>
            </li>
            <li>
                <h6 class="mb-5">Followers</h6>
                <span class="fw-bold">${data.followers}</span>
            </li>
            <li>
                <h6 class="mb-5">Following</h6>
                <span class="fw-bold">${data.following}</span>
            </li>
        </ul>
        <div class="links d-flex between">
            <ul>
                <li>
                    <i class="fas fa-fw fa-map-marked-alt"></i>
                    <span>${(data.location) ? data.location : '-'}</span>
                </li>
                <li>
                    <i class="fas fa-fw fa-link"></i>
                    <span>${(data.blog) ? data.blog : '-'}</span>
                </li>
            </ul>
            <ul>
                <li>
                    <i class="fab fa-fw fa-twitter"></i>
                    <span>${(data.twitter_username) ? data.twitter_username : '-'}</span>
                </li>
                <li>
                    <i class="fas fa-fw fa-building"></i>
                    <span>${(data.company) ? data.company : '-'}</span>
                </li>
            </ul>
        </div>
    </div>
        `
    userCardConatiner.innerHTML = userCradHtml
    }
}

searchForm.addEventListener("submit", (e) => {
  e.preventDefault();
  getuser();
});

```

