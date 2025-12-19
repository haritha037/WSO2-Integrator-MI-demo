# 1-SOA

[https://github.com/NuwanJ/software-architecture-esb-demo](https://github.com/NuwanJ/software-architecture-esb-demo)

[https://www.youtube.com/watch?v=qj6pboedKDE](https://www.youtube.com/watch?v=qj6pboedKDE)

![image.png](images/image.png)

![image.png](images/1af377ad-be68-42ec-90d5-856055deaa6a.png)

When each site directly access each resource/service to manage their content it will be scattered/messy.

![image.png](images/image%201.png)

`SOA (Service Oriented Architecture)` provides a solution for that.

The `ESB (Enterprise Service Bus)` simplifies the mess by centralizing the service access.

This is called integration development.

![image.png](images/image%202.png)

The websites consume the API services through the ESB.

The ESB will do all the mapping, transformations and filtering and provide specific data as responses.

There are different ways of implementing this.

In this lab, we will be implementing like below

![image.png](images/image%203.png)

The APIs on the left will provide the generic data.

The APIs on the right are the `managed APIs` by the ESB and the different research groups can consume them instead of directly interacting with the service APIs and manually doing mapping, filtering and transformation.

They will get only the relevant data for their research group.

# Demonstration

![image.png](images/image%204.png)

Docs - https://mi.docs.wso2.com/en/latest/develop/mi-for-vscode/mi-for-vscode-overview/

WSO2 Integrator: MI offers an extension for Visual Studio Code (VS Code) that simplifies the development of integration solutions.

**MI stands for Micro Integrator**, which is a lightweight, cloud-native, low-code integration platform that helps businesses connect applications, data, and services

# Steps

Install the plugin in VS code

Create a new project

We will be using APIs documented in [https://api.ce.pdn.ac.lk/](https://api.ce.pdn.ac.lk/)

![image.png](images/image%205.png)

We will be implementing the integration for publications

![image.png](images/image%206.png)

API documentation for publications - [https://api.ce.pdn.ac.lk/docs/publications/](https://api.ce.pdn.ac.lk/docs/publications/)

We will be using [https://api.ce.pdn.ac.lk/publications/v1/filter/research-groups/](https://api.ce.pdn.ac.lk/publications/v1/filter/research-groups/) **API endpoint**

![image.png](images/image%207.png)

Create an HTTP endpoint

![image.png](images/image%208.png)

![image.png](images/image%209.png)

![image.png](images/image%2010.png)

Create another endpoint

![image.png](images/image%2011.png)

Each endpoint will get an XML file with its info

![image.png](images/image%2012.png)

Create an API

![image.png](images/image%2013.png)

`/research-groups/v1/{slug}/`

We will be doing it for computer vision website

![image.png](images/image%2014.png)

Right now, these publications are hardcoded in the website.

We are going to create an API service and make it available for the website to use it to fetch latest info instead of hardcoding

![image.png](images/image%2015.png)

Add a resource (publications)

![image.png](images/image%2016.png)

![image.png](images/image%2017.png)

Now, we can define what should happen when the API is called

What we want is to send the part of the response related to vision

![image.png](images/image%2018.png)

SO the ESB should call the service API and filter only the vision related publications.

Click on the + sign

![image.png](images/image%2019.png)

![image.png](images/image%2020.png)

![image.png](images/image%2021.png)

![image.png](images/image%2022.png)

Now we need to `map` the data → click on the + icon on the arrow

![image.png](images/image%2023.png)

![image.png](images/image%2024.png)

![image.png](images/image%2025.png)

![image.png](images/image%2026.png)

![image.png](images/image%2027.png)

Copy paste the payload of - [https://api.ce.pdn.ac.lk/publications/v1/filter/research-groups/](https://api.ce.pdn.ac.lk/publications/v1/filter/research-groups/)

The app will automatically identify the schema

Define the output schema as well

![image.png](images/image%2028.png)

Now map the input and output

![image.png](images/image%2029.png)

To get the count value calculated, add a `Sub Mapping`

![image.png](images/image%2030.png)

![image.png](images/image%2031.png)

Now, return the output as a response

![image.png](images/image%2032.png)

![image.png](images/image%2033.png)

We can also add a `Log` component

Then, do the same for the `/publications` API as well.

![image.png](images/image%2034.png)

add `call endpoint`

![image.png](images/image%2035.png)

Add `data mapping`

![image.png](images/image%2036.png)

Input

```python
{
  "status": "success",
  "data": {
    "code": "computer-vision",
    "name": "Computer Vision",
    "metadata": {
      "description": "We are a research group consisting of faculty, students and external collaborators trying to push the boundaries of computer vision theory and applications.",
      "primary_contact_person": "roshanr@eng.pdn.ac.lk",
      "maintainer": "roshanr@eng.pdn.ac.lk",
      "page": "https://portal.ce.pdn.ac.lk/download/taxonomy-page/computer-vision",
      "website": "https://vision.ce.pdn.ac.lk/"
    },
    "taxonomy": "research-groups"
  }
}
```

Output (metadata is not an object - the fields are flattened

```python
{
  "status": "success",
  "data": {
    "code": "computer-vision",
    "name": "Computer Vision",
    "description": "We are a research group consisting of faculty, students and external collaborators trying to push the boundaries of computer vision theory and applications.",
    "primary_contact_person": "roshanr@eng.pdn.ac.lk",
    "maintainer": "roshanr@eng.pdn.ac.lk",
    "page": "https://portal.ce.pdn.ac.lk/download/taxonomy-page/computer-vision",
    "website": "https://vision.ce.pdn.ac.lk/",
    "taxonomy": "research-groups"
  }
}
```

![image.png](images/image%2037.png)

Let the AI do the mapping → My AI did not work though :(

![image.png](images/image%2038.png)

Add the response

![image.png](images/image%2039.png)

Run the application

![image.png](images/image%2040.png)

Test the endpoint

![image.png](images/image%2041.png)
