---
title: "This is article provides a high level description of Gatsby"
description: "This is article provides a high level description of Gatsby. It contains information about the necessary knowledge and tools needed to build a Gatsby site. It also provides some information on React components and and also on plugins used in a Gatsby site."
date: "2022-07-02"
banner:
  src: "../../images/ilya-pavlov-OqtafYT5kTw-unsplash.jpg"
  alt: "Gatsby Overview"
  caption: 'Photo by <u><a href="https://unsplash.com/photos/OqtafYT5kTw">Ilya Pavlov</a></u>'
categories:
  - "Front End"
  - "Gatsby"
keywords:
  - "Gatsby"
  - "React"
  - "GraphQL"
  - "Markdown"
---

## Introduction

**<u>Name:</u>** Gatbsy

**<u>Type:</u>** Javascript Framework

**<u>Architecture / Technology:</u>**
  - Built on top Node.js 
  - React based components
  - GraphQL for data layer

**<u>Purpose:</u>** Generate static sites (with great SEO capabilities) using Markdown documents, MDX files and Images along with plugins (2500+).

---

### Necessary knowledge and tools

Gatbsy provides starter template that are helpful to quickly create blog sites, portfolio. 
A little secret - this site is built with one of them. Click <u>[here](https://www.gatsbyjs.com/starters/)</u> to see more starter template.

However it is necessary to have a basic understanding of HTML, CSS, Javascript, React and GraphQL (to a small extent).
Node.js and the Gatbsy command line interface are required to create and run a Gatsby application locally.

In order to deploy a gatsby site online, Gatsby offers cloud hosting capabilities with a free tier and different paid tiers.
A Gatbsy coud account is required for this. The Gatsby cloud account can be integrated with a github account (or other source control site like Bitbucket, Gitlab,..) so that every changes committed to a repo can be automatically built and deplpoyed.

### React Components in a Gatbsy site

Gatsby site are built using React components. As a quick reminder, a React component is a function that returns a React element (Object rendered as a DOM element).
Altough components from other packages can be used in a Gatbsy site, there are two main type of components: 
- A page component that can be used as a page (For example Home page, About page...)
- A Building-block / reusable components (For example a Layout component, Sidebar component...) 

The components in a Gatsby site can be styled with CSS just like in a React application. 
They can receive props and also used the "children" prop to create a wrapper component.

### Gatsby plugins

Somtimes we need certain funtionalities to enhance our site. We could build the functionalities we need from scratch. However, this might take some time and effort.
This where plugins come in handy.

Gatsby has a large ecosystem of plugins with thousand of prebuilt packages. 
The plugins are available in the <u>[Gatbsy plugin Library](https://www.gatsbyjs.com/plugins)</u>

Newly installed plugins can be configured in the gatsby-config.js file.


### Gatsby Data Layer
In order to dynamically pull data in a react component, Gatbsy has a feature called the "data layer".
The **data layer** allows data to be dynamically pulled from any source and displayed in the component.

A site may require data from one or multiple sources (file system, database, API, etc...).
A **source plugin** is used to pull the data and store it all in the data layer. A plugin is specific to the source where the data is needed. 
For instance, the **"gatsby-source-filesystem"** is used to pull data from the file system.

*GraphQL* queries are then used in order to get data from the data layer into a component. 
The GraphiQL web browser explorer is a very helpful tool to write and test GraphQL queries before adding them to the component that requires it.

### MDX
There are instances where the data from a source plugin is not always usable unless it is transformed.
For instance, using the **"gatsby-source-filesystem"** plugin can give us information about the files we want to pull data from. 
However, it doesn't allow the data layer to have access to the content of the file. Transformer plugins can help resolve this issue.
A common use case in a Gatbsy site is the use of MDX files to add content to a page. In this instance, 
The **"gatsby-transfomer-mdx"** is used to render the content of an MDX file in a Gatbsy site.

In the data layer, information is stored in objects called nodes. The transformer plugin converts the data in the nodes from one type to the other.

For instance, the gatsby-source-filesystem will create file nodes. The gatsby-transfomer-mdx will transform the file nodes that have the .mdx extension into mdx nodes. 
GraphQL queries can then be written to retrieve the content of an MDX file. 

An "MDXRenderer" component (provided by the gatsby-transfomer-mdx) is used to display content from the MDX nodes.

Want to learn more? Click <u>[here](https://www.gatsbyjs.com/docs/tutorial/)</u> to see short and very helpful hands-on tutorial tutorial is available on the Gatbsy documentation site. 