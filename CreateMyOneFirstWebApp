My Fisrt Web App
===============


So now we wan to crate a new app.

We need some code.

So we will write 2 files one .json and one .ls

Fisrt create a new directory.
	mkdir < name of dir >
	mkdir simpleweb

Second create the new files

package.json

```
{
 "dependencies": {
   "express": "*"
 },
 "scripts": {
   "start": "node index.js"
 }
}
```

index.js

```
const app = express();


app.get('/', (req, res) => {
  res.send('Hi there');
});


app.listen(8080, () => {
  console.log('Listening on port 8080');
});
```

And a new file Dockerfile

```
#Specify a base image
FROM node:alpine


#Insatll some dependencies
COPY ./ ./
RUN npm install


# Default command
CMD ["npm", "start"]
```

Now run the docker build command

	```
	docker build .
	```

```
Sending build context to Docker daemon  4.096kB
Step 1/4 : FROM node:alpine
 ---> 4c6406de22fd
Step 2/4 : COPY ./ ./
 ---> 5a25da3f99ab
Step 3/4 : RUN npm install
 ---> Running in c6a58ad14d36
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN !invalid#2 No description
npm WARN !invalid#2 No repository field.
npm WARN !invalid#2 No license field.

added 50 packages from 37 contributors and audited 126 packages in 2.326s
found 0 vulnerabilities

Removing intermediate container c6a58ad14d36
 ---> c73af6d4311f
Step 4/4 : CMD ["npm", "start"]
 ---> Running in f695c26424e0
Removing intermediate container f695c26424e0
 ---> 7b7374f4bc43
Successfully built 7b7374f4bc43
```

The warning are ok we have no problem now we build our image.

Now tag the image with a name so you can remember.

	```
	 docker build -t myfirstwebapp/simpleweb .
	```

Now run the image.
	
	```
	docker run myfirstwebapp/simpleweb
	```

You will see:

```
> @ start /
> node index.js

Listening on port 8080
```

But now you will not able to reach the application.
So you need to do a port mapping.

So to do this you need to execute the command:
	
	```
	docker run -p 8080:8080 myfirstwebapp/simpleweb
	```

This command say:
	
	```
	docker run -p < the port from outside world >:< forward to a port inside world > name of image
	```

But you don't need to have the same port on two sections:
	
	```
	docker run -p 5000:8080 myfirstwebapp/simpleweb
	```

Now go to your bawser and type:
	
	```
	lockalhost:8080
	```

	OR

	```
	lockalhost:5000
	```


Ok now is a good idea to copy your files on an directory inside to you image.
So do not have conflicts with existing files and your new files.

If you want to see your image shell and see all your root dirs you need to do:

	```
	docker run -it myfirstwebapp/simpleweb sh
	```

Now you can see alla your root files also you can see your files you have crate.
In this case: Dockerfile, index.js, package.json.


Now if you want to copy your new files to a new dir you need to exit from your image sh ( shell ).

To do this you need to tipe the command ```/ # exit```

Now you need to go to your Dockerfile.

	```
	vim Dockerfile
	```

And put this line of code:

	```
	WORKDIR /usr/app
	```

Now your Dockerfile need be like:

```
#Specify a base image
FROM node:alpine


WORKDIR /usr/app

#Insatll some dependencies
COPY ./ ./
RUN npm install


# Default command
CMD ["npm", "start"]
```

Save and exit form the Dockerfile.

Now you need to buld again your image.

	```
 	docker build -t myfirstwebapp/simpleweb .
	```

Then run your image.

	```
	docker run -p 8080:8080 myfirstwebapp/simpleweb
	```
See the app run ok.

```
> @ start /usr/app
> node index.js

Listening on port 8080
```

Open an new terminal window. Now we want to see where our files are.
So we can go to sh of our image.

	```
	docker run -it myfirstwebapp/simpleweb sh
	```
Now we are to the directory where your files are copied.
If you do ```ls``` you will see your files.
 
	```
	/usr/app #ls
	```

If you want to change one of your source code files you will not be able to see the
changes.

For example if you want to change your ```index.js``` file an you want to put a new
message like:

```
const express = require ('express');


const app = express();


app.get('/', (req, res) => {
  res.send('Bye there'); // Before res.send('Hi there'); 
});


app.listen(8080, () => {
  console.log('Listening on port 8080');
});
```

Now if you go to your browser and refresh your page you will see that the message
is not changed.

So that you have to do is to rebuild your image again.

	```
	docker build -t myfirstwebapp/simpleweb .
	```

And now run again your image.

	```
	docker run -p 8080:8080 myfirstwebapp/simpleweb
	```
Now go to your browser and refresh the page.


If you want to do the preveus wark more efficient you need to go to the index.js

	```
	vim index.js
	```

Go and again the message.

```
const express = require ('express');


const app = express();


app.get('/', (req, res) => {
  res.send('How are you doing');
});


app.listen(8080, () => {
  console.log('Listening on port 8080');
});
```


Then go to your Dockerfile and put one more COPY command.

	```
	vim Dockerfile
	```

```
#Specify a base image
FROM node:alpine


WORKDIR /usr/app

#Insatll some dependencies
COPY ./package.json ./ # You need to insert to your code this line
RUN npm install
COPY ./ ./ # And this line

# Default command
CMD ["npm", "start"]
```

In this way you will not build again the files there are not changed.

Now go to your teminar build again the image as we see beffore.

Run your image. And go to your browser and refresh the page.
