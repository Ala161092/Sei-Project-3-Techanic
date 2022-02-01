# TECHANIC

# Overview

This was my first group project from the General Assembly Course. We were given 8 days to build a full stack MERN app with crud functionality and multiple front-end pages. 
As a group we collectively decided on a developer based collab app, where users are able to post projects that they are working on and would like other developers to collaborate with them on. 

Group Members:
Ideen Sanei
Octavie Ploye


# Brief
  • 8 days to plan, build and test our project. Ensuring our ideas are achievable and focusing on creating a professional end product.  
  • A full stack application with our own Front End and Back End.  
  • Use an Express API to serve our data from a Mongo database.  
  • Consume our API with a separate frontend built with React, including but not limited to 2 pages.   
  • Be a complete product with multiple relationships and CRUD functionality for multiple models.  
  • Implement thoughtful user stories/wireframes for MVP and additional features.  
  • Ensure the design of the application is up to industry standards.  

# Site Preview

![Screenshot 2022-02-01 at 15-02-03 Techanic](https://user-images.githubusercontent.com/77459312/151993280-b1fb28b2-93b5-4de8-a808-f77ae359ce73.png)


# Deployment

Job-app can be found here. To gain full access to the site please use the login details 
username: ala@email.com 
password: pass

You are also able to register a new account if you wish to do so.

## Link

- Live Site URL: (https://techanic.herokuapp.com/)

# My process

## Built with

#### Front-End:
- JavaScript(ES6)
- React.js 
- HTML5
- CSS3 + SASS

#### Back-End:
- MongoDb
- Mongoose
- Express
- Node.js

#### Dependencies:
- Cloudinary
- React-Map-Gl
- React Geocode
- Axios

#### Dev Tools:
- GitHub
- Figma
- Notion
- Photoshop
- Firefox Developer Edition
- Insomnia



# Getting Started
 ### Planning
As a team we used Notion to organise and manage the project, this enabled us to allocate tasks to each individual and have a visual representation of the status of each task. Throughout the project week, we communicated through daily morning stand ups on Zoom and would post any relevant information on our Slack group-chat. 
<img width="1349" alt="Screenshot 2021-12-26 at 12 46 31" src="https://user-images.githubusercontent.com/77459312/151993535-fc576b91-ca58-41be-87b5-1f7bbf11f0b2.png">

Once we were all happy with the final project idea and had solidified what our minimum viable product would be, we started on the wire frames of the project.
<img width="900" alt="Screenshot 2021-12-27 at 14 04 14" src="https://user-images.githubusercontent.com/77459312/151993702-ceb70350-f023-4c6d-8553-0e50fb0d9de8.png">

# Development

## Back-End
As a group we decided to work collectively on the back-end functionality of the project. 
Our first task was to create our Job posting model. We needed to ensure we knew exactly what information we wanted to display as well as how the information would be displayed

```
const postSchema = new mongoose.Schema(
{
   project: { type: String, required: true, maxlength: 50 },
   experience: { type: String, required: true, maxlength: 200 },
   description: { type: String, required: true, maxlength: 600 },
   technologies: [{ type: String }],
   location: {type: String},
   owner: { type: mongoose.Schema.ObjectId, ref: 'User', required: true },
   comments: [commentSchema],
   saved: [saveSchema],
 },
 {
   timestamps: true,
 }
)

```

Once we had the majority of our models and had established the relationships between them, we were able to work on our endpoints - and start testing out the CRUD functionality on insomnia. 
At this point, we wanted to focus mainly on the endpoints for the post schema and user schema, ensuring that we were able to delete, get and post new user profiles as well as job listings. 

```
router
 .route('/profile')
 .get(secureRoute, getProfile)
 .put(secureRoute, updateProfile)
 
router.route('/profiles').get(getAllProfiles)
 
router.route('/posts').get(getAllPosts).post(secureRoute, addAPost)
router
 .route('/posts/:id')
 .get(getSinglePost)
 .delete(secureRoute, removePost)
 .put(secureRoute, updatePost)
```

![Screenshot 2022-02-01 at 14 35 03](https://user-images.githubusercontent.com/77459312/151988256-bc3cab6f-32eb-4f16-8d46-f8f49f3d9597.png)


## Front-End
As we were approaching the final few days of the project deadline, we decided it would be more efficient if we split up the workload. 

One of the features of the site includes users being able to create their own profile, where they can upload an image and some bio. Once the profile is created the user also has the ability to make changes to it. 

https://user-images.githubusercontent.com/77459312/151989429-86b80208-c315-4c12-9af6-6c2f931d9f9e.mov

```
const ProfileUpdate = () => {
  const navigate = useNavigate();
  const [formData, setFormData] = useState({
    bio: "",
    skills: "",
    experience: "",
    profileImage: "",
    saved: "",
  });

  useEffect(() => {
    getProfile().then(setFormData);
  }, []);

  const handleSubmit = async (e) => {
    e.preventDefault();
    const token = localStorage.getItem("token");
    const newData = { ...formData };
    try {
      const updatedData = await axios.put("api/profile", newData, {
        headers: { Authorization: `Bearer ${token}` },
      });
      navigate("/profile");
      console.log("UPDATED PROFILE DATA - ", updatedData);
    } catch (err) {
      console.log(err);
    }
  };

  const handleChange = (e) => {
    const newFormData = { ...formData, [e.target.name]: e.target.value };
    setFormData(newFormData);
  };

  const handleImageUrl = (url) => {
    setFormData({ ...formData, profileImage: url });
  };

```
Ideen was able to apply many React components such as React Select, users are able to select the different technologies and it would render as an individual badge on the post. 

https://user-images.githubusercontent.com/77459312/151991839-324320a9-e029-4370-84d4-8ef3463e3c64.mov

```
const selectOptions = [
  { value: 'React', label: 'React' },
  { value: 'Javascript', label: 'Javascript' },
  { value: 'MongoDB', label: 'MongoDB' },
  { value: 'HTML5', label: 'HTML5' },
  { value: 'React', label: 'React' },
  { value: 'CSS', label: 'CSS' },
]

const PostAdd = () => {
  const [formData, setFormData] = useState({
    project: '',
    experience: '',
    description: '',
    technologies: [],
  })

  const navigate = useNavigate()

  const handleChange = (e) => {
    const { name, value } = e.target
    setFormData({ ...formData, [name]: value })
  }

```
### React Mapbox
Working alone on the map feature of our application, I was able to learn and further cement my knowledge of React. Using the Mapbox API, which shows the location of the multiple job posts on site, users are able to click on any popup and view the post in full. 
To further enhance this feature I also used the opencage data geocoding API. This allows users to either enter a postcode/county when adding a post. The geocoding API then converts this into latitude and longitude coordinates, which then populates itself on the map. 

https://user-images.githubusercontent.com/77459312/151995040-58f8d28f-08c9-445f-8c72-8bef8af3db06.mov

```
const Simplemap = ({ _id }) => {
 const [coords, setCoords] = useState([]);
 const [postId, setPostId] = useState();
 const navigate = useNavigate();
 useEffect(() => {
   async function getGeoCode() {
     const { data } = await axios.get("/api/posts");
     const posts = data;
     const newCoords = [];
     const posting = [];
     for (let i = 0; i < posts.length; i++) {
       const location = posts[i].location;
       const id = posts[i]._id;
       const response = await axios.get(
         `https://api.opencagedata.com/geocode/v1/json?q=${location}&key=39b07783561747b1b301c84b5e602c31&language=en&pretty=1`
       );
       const longlat = response.data.results[0].geometry;
       const complete = { ...longlat, id };
       newCoords.push(complete);
       posting.push(id);
     }
     setCoords(newCoords);
     setPostId(posting);
   }
   getGeoCode();
 }, []);

```

### Cloudinary
Using the Cloudinary API, I was able to create a custom image upload component for a user's profile. This task took a lot of prep beforehand - as I had never worked with cloudinary before, I was able to spend a few hours reading the documentation so that I could thoroughly understand the process. 

```
const uploadUrl = "https://api.cloudinary.com/v1_1/dmbnpx1fy/image/upload";
const uploadPreset = "vpcxdek0";
console.log("preset---->", uploadPreset);
console.log("uploadUrl---->", uploadUrl);
 
export const ImageUpload = ({ handleImageUrl, value }) => {
   const handleUpload = async (event) => {
     const data = new FormData()
     data.append('file', event.target.files[0])
     data.append('upload_preset', uploadPreset)
     const res = await axios.post(uploadUrl, data)
     handleImageUrl(res.data.url)
   }
    return (
     <>
       {value ? (
         <div className='image-field'>
           <img src={value} alt='Profile Image' />
         </div>
       ) : (
         <div className='image-field'>
           <label>Profile Image: </label>
           <input className='input' type='file' onChange={handleUpload} />
         </div>
       )}
     </>
   )
 }

```

# Challenges
• One aspect of the project which I struggled with at first was learning how to use Git as a team. It took some time to fully understand the different steps of       creating multiple branches and then resolving merge conflicts. However, this is something I am now very comfortable with. 

# Future Improvments
  • Moving forward, we would have added a contact feature that allows the developers to reach out to the prospective job owner.  
  • Although the save functionality is working on the back-end, we are currently unable to see any changes on the front-end. With more time we would have liked to     have worked out a way to get this working.   
  • With more time, I would have liked to incorporate the React-Bootstrap CSS framework into the project.  

# Wins & Key Learnings:
  • Working as a team we were successfully able to collaborate together and produce a fully functioning full-stack MERNapp.     
  • Learning how to handle and resolve git merge conflicts, git branches creation and checkout.   
  • Learning and guiding each other through issues we individually or collectively had was the biggest win, as we were able to learn from each other's knowledge.  


  

