# Headless Wordpress website deployed using Netlify and Wordpress (AWS Lightsail)

Deploy a Next.JS project using Netlify to host the frontend and Wordpress as the backend. This is a Headless Wordpress website, the goal is to decouple the frontend from the backend, to make it easier for developers to evolve the website and for non-developers to update its content using Wordpress CMS interface.

Frontend: 
- Netlify
- Build Hooks

Backend: 
- AWS Lightsail instance running Wordpress + MariaDB  
- Wordpress Plugins: WP GraphQL + WP Webhooks

## Demo

### [https://headless-wp-dog.netlify.app](https://headless-wp-dog.netlify.app)


## Configuration

### Step 1. Create WordPress site

- Create an AWS Lightsail instance with the Wordpress blueprint. Once it's created, access it using the resulting public IP. Example: http://34.228.233.81/wp-admin.
- In order to login, access the instance via SSH and get the credentials on /home/bitnami/bitnami_application_password file. The user will be 'user'.
- Install WP GraphQL plugin and activate it. GraphQL will appear on the right-side menu of the Wordpress Dashboard. Click on Settings and copy the GraphQL endpoint. Example: http://34.228.233.81/graphql.
- Install WB Webhooks plugin. The goal is to configure build hooks from Netlify, so when a post is created/updated/deletes (among other triggers), Netlify updates the website automatically.


### Step 2. Deploy site on Netlify 

- On Sites menu of your Netlify account, create on Add new site, then Import an existing project.
- Connect Github account and choose this project. 
- Insert `npm run build` on Build command and create an Environment Variable called `WORDPRESS_API_URL`. Paste the GraphQL endpoint from step 1 as this variable value.
- Then, run the deploy.


### Step 3. Connect frontend (Netlify) and backend (Wordpress)

- On your website page on Netlify, go to `Site settings` in the menu. Then choose `Build & deploy` and go to `Build hooks` area.
- Click on `Add build hook`.
- Choose a name (Wordpress, for example), the branch to build (would be main in this case) and Save. Copy the Webhook URL.
- Now access your Wordpress Dashboard, click on Settings, then click on WP Webhooks.
- Choose `Send Data` on the WP Webhooks menu. Choose which actions you'll need to trigger website update on Netlify (for example, Post created).
- Click on `Add Webhook URL`, choose a name (Netlify, for instance) and paste the Webhook URL from the Build hook created on Netlify.


### Step 4. Test creating new content

Inside your WordPress admin, go to **Posts** and start adding new posts:

- Create a new post and hit **Publish**.
- Inside your Netlify dashboard, you should see a new build of the website triggered by this new post.
- Acess your website URL and check if the new post is on.

Ok, you have a Headless Wordpress site now. For developers, when they push new code to the Github main branch, Netlify will trigger a new website deploy. For non-developers, when they create/update/delete a post on the Wordpress CMS interface, this will also make the website to be updated automatically.


