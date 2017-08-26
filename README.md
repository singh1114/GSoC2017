# Google Summer of Code 2017

## Ranvir Singh - Web app for online code scanning

Welcome to the project page of my GSoC 2017 project!

The goal of my project was to build a web app to scan the code for information like licenses, authors, packages and other such information. The library was already implemented by the [community](https://github.com/nexb/scancode-toolkit) and my job was to create a web app on the top of it, so that the people can directly scan their code from the web app. Here is the link to whole of my code during the GSoC [https://github.com/nexB/scancode-server/commits/develop?author=singh1114](https://github.com/nexB/scancode-server/commits/develop?author=singh1114). 

The idea was to create the ReST API using the [Django's rest framework](http://www.django-rest-framework.org/). Most of the work related to the project written in the proposal is done and the product is ready to be used after some minor changes. [Here](https://github.com/nexB/scancode-server/commits/develop?author=singh1114) is the list of all my commits during the GSoC-2017.

I documented my progress in weekly blog posts. These can be found in my [blog](https://ranvirsinghprojects.wordpress.com/category/gsoc-2017/).

Now, I am going to discuss them according to the different phases of the project:


## Phase 1: Creating models

This was the most important part of the project as everything was dependent on this. Even one mistake in this part could have left us in a very bad situation. Minor changes did happen in the models from time to time. I wrote a [blog post](https://ranvirsinghprojects.wordpress.com/2017/06/13/models-for-the-scancode-project-app/) about the models by which we started the journey. 


## Phase 2: Creating forms

This was another basic and important part of web app when created using Django. Using Django forms gave us the opportunity to use the forms to their maximum usage. We also added the feature to add the code from the local i.e. uploading the code from the local to scan. On this I found `FileField` that helped us to take the code from the local. I have written a [blog post](https://ranvirsinghprojects.wordpress.com/2017/06/21/filefield-in-forms-to-upload-files-to-the-server/) about this too.


## Phase 3: Using subprocess module

Next phase was to make use of `subprocess` python module as we wanted to run the bash commands in the python code. We extensively used this module in our code and it went on to be the most important module for the success of this project. I have written a detailed description on the way we used this module for the development for our project. See the [blog post](https://ranvirsinghprojects.wordpress.com/2017/06/21/a-word-about-subprocess-module/)


## Phase 4: Using celery to run tasks asynchronously

The task of scanning is a time consuming task. As the web app must not allow the user to wait for much time, we used `celery` python module to run the tasks in the background. I have written detailed description on how to use this module. [Here](https://ranvirsinghprojects.wordpress.com/2017/06/25/using-celery-to-run-long-running-task-asynchronously/) is the link to the post.


## Phase 5: Testing

After this, my mentor told me to write the unit tests for the code. It was a very time consuming task as I had never written any tests in the past. I was able to write some tests for the code but the task is still not complete. In the beginning, I was not able to find any inspiration about writing the tests so I wrote some initial motivation required to push me toward writing the tests. [Here](https://ranvirsinghprojects.wordpress.com/2017/07/06/writing-unit-tests-for-the-models/) is the blog post for this.


## Phase 6: Serialization

In this, I took the data from the Django models and tried to convert them into more easily transferable data i.e. `JSON` format. We still have to do more things in this part. I have written a detailed description of this in the following [blog post](https://ranvirsinghprojects.wordpress.com/2017/07/22/serialization-a-week-long-struggle/)

