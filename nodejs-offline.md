---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-12"
subcollection: "Nodejs"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Work offline with Node.js
{: #offline_mode}

When a Node.js application is pushed to {{site.data.keyword.Bluemix}}, the SDK for Node.js buildpack
typically downloads artifacts from external resources such as Node modules from NPM.  In some
situations, such as with  [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated) and
[{{site.data.keyword.Bluemix_local_notm}}](/docs/local/index.html#local),  you can customize access to sites external to {{site.data.keyword.Bluemix_notm}}.  
{: shortdesc}

The Node.js buildpack can access the following external sites. You might need to *whitelist* these sites in [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated) and
[{{site.data.keyword.Bluemix_local_notm}}](/docs/local/index.html#local) {{site.data.keyword.Bluemix_notm}} environments.

* http://nodejs.org/ may be used to ascertain available node engine versions.
* https://s3pository.heroku.com is used to retrieve Node engine versions not included in the buildpack.
*  https://www.npmjs.com/package/npm and https://semver.herokuapp.com are used to retrieve versions of npm not included in the buildpack.
* https://iojs.org is used to retrieve older version of node which are either not contained in the buildpack or not available at  https://semver.herokuapp.com.
* https://registry.npmjs.org is used to retrieve node modules such as express.

To minimize the set of whitelisted sites, configure your applications to use a Node engine version which is included in the SDK for Node.js buildpack.  See [latest updates](/docs/runtimes/nodejs/updates.html) for the set of Node engine versions included in the buildpack.  If you configure your application to use these Node engine versions, then only the https://registry.npmjs.org site is required to download modules.

Be aware that when new versions of the SDK for Node.js buildpack are installed, the set of available engine versions often changes to more recent versions.  You might be required to reconfigure your app to specify a more recent Node engine version included in the buildpack.


## Offline applications
{: #offline_applications}

To eliminate the need to access https://registry.npmjs.org you can include all the Node modules your application requires within your application.  To do this run `npm install` for all of the required application modules, and then include the resulting `node_modules` directory with your pushed application.

Your dependencies can have dependencies, which have dependencies and so on, but the `package.json`
only contains the top-level dependencies. If one of the dependencies uses a range in the package.json and a new version of that is released, then modules in your `node_modules` directory can become obsolete. *Shrinkwrap* helps you lock down all the dependency versions so this can't happen.  To use shrinkwrap, start with an empty or clean `node_modules` directory. Then, in your project's root directory run the following commands:

```
npm install
```
{: codeblock}

```
npm dedupe
```
{: codeblock}

```
npm shrinkwrap
```
{: codeblock}

This may change your `package.json` and add `npm-shrinkwrap.json` in your root directory.
Whenever you make a change to dependencies in the `package.json` file , rerun the `npm dedupe` and `shrinkwrap` commands.

## Working with a proxy
{: #working_with_proxy}

In some environments such as [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated) and
[{{site.data.keyword.Bluemix_local_notm}}](/docs/local/index.html#local), a proxy can be configured. See
[Working with a proxy](/docs/runtimes-common/workingWithProxy.html) for more details.
