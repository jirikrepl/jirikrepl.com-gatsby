---
title: Meteor acceptance testing
date: "2016-08-10T23:46:37.121Z"
layout: post
path: "/2016/08/10/Meteor-acceptance-testing/"
category: "Meteor"
description: "Acceptance tests tests the features of your app.
                Those features are tested it the same way as users would use those features.
                We want to automate (as oppose to manual testing) this process."
---

*Acceptance tests* tests the features of your app. Those features are tested it the same way as users would use those features.
We want to automate (as oppose to manual testing) this process. So test will start browser, which interact with your app's UI in the same way as any user.

Sounds simple enough, right? But when you writing your tests, you can create technology debt even without knowing it.
Before you start reading or after you finish, consider reading [Testing Success Factors](https://github.com/xolvio/automated-testing-best-practices/blob/master/content/TESTING-SUCCESS-FACTORS.md) from [Xolv.io](http://xolv.io/) - masters of testing.      

We will need some software tools. I will list those here, then I will dive into installation / usage details down below.

* [Chimp](https://chimp.readme.io/) - tests will be run with *Chimp* 
* [Mocha](https://mochajs.org/) - test will be written with in this framework
* [Chai](http://chaijs.com/) - assertion library, it will help us assert test outputs with defined values
* [Webdriver.io](http://webdriver.io/) - it will control the browser 


## Creating tests  
In your Meteor app create directory called `tests`. Any directory with name `tests` is not loaded by Meteor. See [Meteor special directories](https://guide.meteor.com/structure.html#special-directories).

Firstly we will need to abstract information about *HTML page (under test)* with *page object pattern*. Then we will create the *test file* itself.
Before we start let's get familiar with *page object pattern* first.

## Page object pattern

*Page object* is a class which encapsulates logic of particular page. It stores DOM selectors and returns elements defined by these selectors.
The reason why to have *page object* is whether UI changes, only pageObject should be rewritten and then tests should still work.
It is *DRY*, because *selectors* are in one place in *page object*, and not in many places in tests.
With *page object* test can be much explicitly written and therefore much simpler to understand.

### More on page objects
Martin's Fowler [page object](http://martinfowler.com/bliki/PageObject.html)
*Page object* in [webdriver.io](http://webdriver.io/guide/testrunner/pageobjects.html), ES5 code only:(

### Page object and test Files   
*Note that these three files down below are just example files. You cannot get test working only by copying these files. You would also have to write down exact HTML code of tested page.*   

#### page.js
*page.js* is a base class which other *page object* classes will extend.
In this particular code to get `open()` method working, you need to install `url` module:

    $ npm install --save-dev url
    
More on using [npm with Meteor](https://guide.meteor.com/using-npm-packages.html).

```javascript
// page.js

import urlModule from 'url';

export default class Page {

    // opens any of given relative path
    open(path) {
        browser.url(urlModule.resolve(process.env.ROOT_URL, path));
    }
}
```
    
#### lesson.page.js    
*lesson.page.js* extends *page.js*. It can contain any logic of real lesson's *HTML* page.
In this code `browser.element()` refers to webdriver.io instance and *element()* method. In Chimp `browser` object is global, you don't need to import anything.
 
Note also that in [webdriver.io](http://webdriver.io/api/protocol/element.html): `client === browser`

```javascript
// lesson.page.js

import Page from './page';

export default class LessonsPage extends Page {

    open() {
        super.open('kurz-zakladni');
    }
    
    get newLesson() {
        return browser.element('#new-lesson');
    }

    get lessonNameInput() {
        return browser.element('#lesson-name-input');
    }
    
    get newLessonOK() {
        return browser.element('#new-lesson-ok');
    }
        
    get lessonName() {
        return browser.element('#lessonName')
    }
}
```

#### lessonTest.js
Finally our test file `lessonTest.js`. We use [mocha's](https://mochajs.org/#getting-started) `describe()` and `it()` functions.

```javascript
// lessonTest.js

import LessonsPage from './pageObjects/lessons.page';

describe('On essons page', () => {
    let lessonPage = new LessonsPage();
    let lessonName = 'Test lesson 1'
    
    // this test has @dev annotation
    // it will be run in Chimp --watch mode
    // more details below
    it('you should create new lesson @dev', () => {
        lessonPage.open();
        lessonPage.newLesson.click();
        lessonPage.lessonNameInput.setValue(lessonName);
        lessonPage.newLessonOK.click();
        
        // assert result with Chai
        lessonPage.lessonName.should.equal(lessonName);
    });
});
```
    
## Chimp
We will run tests with [Chimp](https://chimp.readme.io/). It elegantly integrates with Meteor. Install it globally with npm:

    $ npm i -g chimp
    
#### Run tests
Firstly start your Meteor app on port 3000 (default port).

    $ cd my-meteor-app
    $ meteor
    
Then run tests with Chimp. Remember have your Meteor app running and provide the correct path to tests with `--path` flag.  

    $ chimp --ddp=http://localhost:3000 --watch --mocha --path=chimp/tests

Witout `--browser` flag Chrome is set as [default browser](https://github.com/xolvio/chimp/blob/master/src/bin/default.js#L38). Use flag `--browser` to run tests in other browser.

With `--watch` flag only tests with `@dev` annotation are run. Without `--watch` all test are run *once*.

#### Optional webdriver.io configuration file

Create config file to change behavior of *webdriver.io*.

It can be handy to change time limit for [waitForVisible()](http://webdriver.io/api/utility/waitForVisible.html) method. Default time limit is 500 ms, which can be sometimes too short.

```javascript
// config.js
    
// it has to be ES5 export, otherwise it would not work with Chimp
module.exports = {
    webdriverio: {
        waitforTimeout: 2000
    }
};
```

Pass *config.js* as a first argument to *Chimp*.

    $ chimp chimp/config.js --ddp=http://localhost:3000 --watch --mocha --path=chimp/tests
        
## Conclusion
With help of page object and other smart technologies we can maintain our acceptance test base with more ease. Test will become less fragile.
Test can be run after each commit locally, which could be annoying after while. Better way is to set up CI environment to run tests there. In my next article, I want to explore this option as well.
           
Do you want to improve [this article](https://github.com/jirikrepl/jirikrepl.com-hexo/blob/master/source/_posts/Meteor-acceptance-testing.md)? Pass me a pull request.           