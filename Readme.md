Server Deployment steps
comment await commands outside api methods for solving gateway timeout error
//comment following commands
await client.connect();
await client.db("admin").command({ ping: 1 });
create vercel.json file for configuring server
{
"version": 2,
"builds": [
{
"src": "index.js",
"use": "@vercel/node"
}
],
"routes": [
{
"src": "/(.\*)",
"dest": "index.js",
"methods": ["GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS"]
}
]
}
Add Your production domains to your cors configuration
//Must remove "/" from your production URL
app.use(
cors({
origin: [
"http://localhost:5173",
"https://cardoctor-bd.web.app",
"https://cardoctor-bd.firebaseapp.com",
],
credentials: true,
})
);
Let's create a cookie options for both production and local server
const cookieOptions = {
httpOnly: true,
secure: process.env.NODE_ENV === "production",
sameSite: process.env.NODE_ENV === "production" ? "none" : "strict",
};
//localhost:5000 and localhost:5173 are treated as same site. so sameSite value must be strict in development server. in production sameSite will be none
// in development server secure will false . in production secure will be true
