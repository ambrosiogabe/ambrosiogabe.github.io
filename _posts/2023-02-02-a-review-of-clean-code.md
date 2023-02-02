---
layout: post
title:  "Why are people still recommending this book?"
date:   2022-02-02 12:00:00 -0500
categories: [code]
tags: [clean code, bob martin, code]
cover: /public/images/cleanCodeReview/bookCover.jpg
---

I decided to \*ahem\* dive into the ancient holy relic (among corporate programmers) that is called "Clean Code". Below is an unfiltered recounting of my dangerous opinions on the matter. Proceed with caution.

## Intro

Bob Martin starts off with a lengthy discussion about what "clean code" *is*. In this lengthy discussion he doesn't discuss any verifiable metrics that can determine whether a piece of code is "cleaner" than another piece of code. Instead he lists off a bunch of pithy quotes from other "thought leaders" at the time explaining "pie in the sky" notions of what "clean code" is.

For example, clean code is "when each routine you read turns out to be pretty much what you expected". This is clearly measurable and verifiable. Or how about "I could list all the qualities that I notice..but there is no overarching quality that leads to all of them. Clean code always looks like it was written by somebody who cares". These quotes all boil down to, "clean code is what _looks_ and _feels_ good to _me_". In other words, clean code is entirely subjective and has no bearing on the realities that matter like: code performance, code complexity, and code correctness.

He ends this chapter talking about how Jiu Jitsu has many divergent schools. No school is the correct Jiu Jitsu, instead they all have their own flavor. Likewise, he pontificates, there is no correct school of thought for Clean Code, but he's going to school us anyways.

In fact, the only observable metric he gives about clean code in this introduction is the comic on the first page which gives a visualization of "WTFs per minute". I guess we can tell how clean your code is by how much your co-workers say WTF while reading your code. As you can see, this is a foolproof objective metric with which to guide your coding decisions, and it's very easy to measure, and it's always going to be the same regardless of who's WTFing behind the closed door.

![WTFs Per Minute](/public/images/cleanCodeReview/wtfsPerMinute.png)
<p class="image-subtitle">WTFs per Minute</p>

## Chapter 2: Variable Names

After explaining the Zen of "clean code", Bob launches into the meat of the book. Of course, all programs start with names. How to name your files, how to name your functions, how to name your variables, and on and on. So it's a natural place to begin talking about code "cleanliness".

I mostly agree with everything Bob says in the second chapter. Names should be expressive and you should allow the type system to add additonal clarity to the intent of your variables. Hungarian notation is outdated. If your language allows custom postfix operators like C++, use those to express your intentions for things like numbers with units or complex types, for example:

```cpp
Color color = "#FF2311"_hexColor;
```

His examples seem to exude a couple of behaviors that I find questionable though. He does no sort of error checking. I get it, this is a book and he wants to keep it succinct, however the first chapter is _a lot_ of unnecessary fluff with no substance, so I don't feel like he was trying hard to keep the book succinct.

In recent days I've taken to very assertive programming. If there's an assertion that should hold true I embed it directly in the code. This allows readers to see what assertions should hold true, and it crashes the program quickly when it encounters undefined behavior.

Here's an example of the lack of error checking in the book:

```java
// P. 23 Uncle Bob's cleaned version, comments mine
// What does this mean? The whole chapter was about clear variable names
// and we're left with this?
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;
// Why is this not 
// for (int j = 0; j < taskEstimate.length(); j++)
for (int j = 0; j < NUMBER_OF_TASKS; j++) {
    // No bounds check here
    int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
    // Is this supposed to be (realTaskDays / WORK_DAYS_PER_WEEK)?
    int realTaskWeeks = (realDays / WORK_DAYS_PER_WEEK);
    sum += realTaskWeeks;
}
```

There's no bounds checking, and it's entirely unclear what he's trying to do here. Further, with a functional paradigm all this boilerplate can be removed and you can express this snippet much more succinctly:

```typescript
const realDaysPerIdealDay = 4;
const WORK_DAYS_PER_WEEK = 5;
const sum = taskEstimate
    .slice(0, NUMBER_OF_TASKS) // Implicit bounds check here
    .reduce(function (sum, currentEstimate) {
        const realTaskDays = currentEstimate * realDaysPerIdealDay;
        return sum + (realTaskDays / WORK_DAYS_PER_WEEK);
    });
```

Finally, the end of this chapter gives us a very questionable example of how to "clean up" a piece of "dirty" code:

```java
// P.28 Clean Code
private void printGuessStatistics(char candidate, int count) {
    String number;
    String verb;
    String pluralModifier;
    if (count == 0) {
        number = "no";
        verb = "are";
        pluralModifier = "s";
    } else if (count == 1) {
        number = "1";
        verb = "is";
        pluralModifier = "";
    } else {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }
    String guessMessage = String.format(
        "There %s %s %s%s", verb, number, candidate, pluralModifier
    );
    print(guessMessage);
}
```

Now aside, from being a bit verbose, this function is very readable and I would argue it's already clean. If I were to change this, I would just remove some of the duplication here by setting sensible defaults for the `String`s.

```java
private void printGuessStatistics(char candidate, int count) {
    String number = Integer.toString(count);
    String verb = "are";
    String pluralModifier = "s";

    if (count == 0) {
        number = "no";
    } else if (count == 1) {
        verb = "is";
        pluralModifier = "";
    }

    String guessMessage = String.format(
        "There %s %s %s%s", verb, number, candidate, pluralModifier
    );
    print(guessMessage);
}
```

This seems reasonable to me, and if you don't care about printing `"no"` instead of `0` when there are 0 elements, then you can remove that first if statement and reduce this function even more.

Bob thinks otherwise, his "clean" version looks like this:

```java
// P.29 Clean Code, Comments are mine
public class GuessStatisticsMessage {
    private String number;
    private String verb;
    private String pluralModifier;

    // Why is this not in the constructor?
    // So you're supposed to use it like this:
    //   String formattedString = new GuessStatisticMessage().make('a', 5);
    //   print(formattedString);
    // Why? We've completely overcomplicated an API that used to be
    //   printGuessStatistic('a', 5);
    public String make(char candidate, int count) {
        createPluralDependentMessageParts(count);
        return String.format(
            "There %s %s %s%s",
            verb, number, candidate, pluralModifier );
    }

    // This function tells me absolutely nothing!
    // It relies on other functions having side effects that magically
    // change the internal state of this object, which means people who read
    // this code later will have to jump into all the other functions to
    // find out how the state of this class actually changes.
    private void createPluralDependentMessageParts(int count) {
        if (count == 0) {
            thereAreNoLetters();
        } else if (count == 1) {
            thereIsOneLetter();
        } else {
            thereAreManyLetters();
        }
    }

    private void thereAreManyLetters(int count) {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }

    private void thereIsOneLetter() {
        number = "1";
        verb = "is";
        pluralModifier = "";
    }

    private void thereAreNoLetters() {
        number = "no";
        verb = "are";
        pluralModifier = "s";
    }
}
```

I'm sorry, but what is this mess? He's taken a piece of code that was trivial in complexity, had 0 side-effects (aside from printing to IO) and was very easy to comprehend, and he's broken it up into a giant stateful object with heavy use of unintuitive mutable functions.

Before, if a user wanted to understand what was happening, they simply had to look at the single function for a few moments and trace the control flow.

Now, you have to jump through 5 different functions and several divergent paths of control flow. Each time you have to jump into a new function, your brain is forced to rebuild a mental "stack" of what the state of the class is. This compounds exponentially with every new function call added.

Additionally, the proficient use of side effects means that you cannot intuititively look at any individual piece of this code and safely assume that it won't modify random state.

Finally, the API is worse! Now, instead of simply calling a function, your forced to create an unnecessary object, and then call a function on the object. Why is this logic stored in a `make` function? Why is it not in the constructor, at least obviating the need for these random function calls from the end-user? This piece of code looks anything but clean to me.

```java
// What's nicer to use? This:
String formattedString = new GuessStatisticMessage().make('a', 5);
print(formattedString);
// Or this?
printGuessStatistic('a', 5);
```

## Chapter 3: Functions

This chapter begins with a function that Bob found in some codebase called FitNesse. This function looks like the following, and Bob asks the reader to analyze the function and try to understand what it does in less than 3 minutes:

```java
public static String testableHtml(
    PageData pageData, 
    boolean includeSuiteSetup
) throws Exception {
    WikiPage wikiPage = pageData.getWikiPage();
    StringBuffer buffer = new StringBuffer();
    if (pageData.hasAttribute("Test")) {
        if (includeSuiteSetup) {
            WikiPage suiteSetup = PageCrawlerImpl.getInheritedPage(SuiteResponder.SUITE_SETUP_NAME, wikiPage);
            if (suiteSetup != null) {
                WikiPagePath pagePath = suiteSetup.getPageCrawler().getFullPath(suiteSetup);
                String pagePathName = PathParser.render(pagePath);
                buffer.append("!include -setup .")
                    .append(pagePathName)
                    .append("\n");
            }
        }
        WikiPage setup = PageCrawlerImpl.getInheritedPage("SetUp", wikiPage);
        if (setup != null) {
            WikiPagePath setupPath = wikiPage.getPageCrawler().getFullPath(setup);
            String setupPathName = PathParser.render(setupPath);
            buffer.append("!include -setup .")
                .append(setupPathName)
                .append("\n");
        }
    }
    buffer.append(pageData.getContent());
    if (pageData.hasAttribute("Test")) {
        WikiPage teardown = PageCrawlerImpl.getInheritedPage("TearDown", wikiPage);
        if (teardown != null) {
            WikiPagePath tearDownPath = wikiPage.getPageCrawler().getFullPath(teardown);
            String tearDownPathName = PathParser.render(tearDownPath);
            buffer.append("\n")
                .append("!include -teardown .")
                .append(teardownPathName)
                .append("\n");
        }
        if (includeSuiteSetup) {
            WikiPage suiteTeardown = PageCrawlerImpl.getInheritedPage(SuiteResponder.SUITE_TEARDOWN_NAME, wikiPage);
            if (suiteTeardown != null) {
                WikiPagePath pagePath = suiteTeardown.getPageCrawler().getFullPath(suiteTeardown);
                String pagePathName = PathParser.render(pagePath);
                buffer.append("!include -teardown .")
                    .append(pagePathName)
                    .append("\n");
            }
        }
    }
    pageData.setContent(buffer.toString());
    return pageData.getHtml();
}
```

Now, I agree with Bob that this is a very poorly written function, but I disagree with the propsed fixes that Bob presents. Bob proposes to fix this method by shortening it to this (I say shortening, but you can see the *actual* entirety of this monstrosity at the end of this section):

```java
public static String renderPageWithSEtupAndTeardown(
    PageData pageData, 
    boolean isTestSuite) 
throws Exception {
    boolean isTestPage = pageDat.hasAttribute("Test");
    if (isTestPage) {
        WikiPage testPage = pageData.getWikiPage();
        StringBuffer newPageContent = new StringBuffer();
        includeSetupPages(testPage, newPageContent, isSuite);
        newPageContent.append(pageData.getContent());
        includeTeardownPages(testPage, newPageContent, isSuite);
        pageData.setContent(newPageContent.toString());
    }

    return pageData.getHtml();
}
```

Now Bob correctly says that you may still not have a clear understanding of what this function does, but he assumes that the intent is at least clearer. I would argue that the intent is just as muddy and my understanding is even less than before. All he's done here is removed the implementation details, and while this can be good when you're exposing a public API to consumers, this is not good when you're trying to improve the readability for active maintainers of your code.

As a maintainer, I want to know what this function is doing, and I don't want to hunt all over the codebase for functions that are used exactly once by this function to "improve" readability. Once again, each function call in this small function requires a context switch from the reader, and the reader additionally has to build a mental stack to keep track of what's happening as they trace the code. I would propose to "clean up" the function more like this:

```java
public static String getPageData(String pageName, WikiPage wikiPage) {
    StringBuffer result = new StringBuffer();
    WikiPage setup = PageCrawlerImpl.getInheritedPage(pageName, wikiPage);

    if (setup != null) {
        WikiPagePath pagePath = setup.pageCrawler.getFullPath(setup);
        String pagePathName = PathParser.render(pagePath);
        result.append("!include -setup .")
            .append(pagePathName)
            .append("\n");
    }
    
    return result.toString();
}

public static String testableHtml(
    PageData pageData, 
    boolean includeSuiteSetup
) throws Exception {
    WikiPage wikiPage = pageData.wikiPage;
    StringBuffer buffer = new StringBuffer();

    boolean isTestPage = pageData.hasAttribute("Test");
    if (isTestPage && includeSuiteSetup) {
        String suiteSetupData = getPageData(SuiteResponder.SUITE_SETUP_NAME, wikiPage);
        buffer.append(suiteSetupData);
    }

    if (isTestPage) {
        String setupData = getPageData("SetUp", wikiPage);
        buffer.append(setupData);
    }

    buffer.append(pageData.content);

    if (isTestPage) {
        String teardownData = getPageData("TearDown", wikiPage);
        buffer.append(teardownData);
    }

    if (isTestPage && includeSuiteSetup) {
        String suiteTeardownData = getPageData(SuiteResponder.SUITE_TEARDOWN_NAME, wikiPage);
        buffer.append(suiteTeardownData);
    }

    pageData.setContent(buffer.toString());
    return pageData.getHtml();
}
```

The first thing I did was remove the unnecessary getters. They add density to the code that hurts the readability. If your language has proper support for mutable vs non-mutable data, you can mark the variables as mutable/immutable to be more explicit with your intentions. This also obviates the need for getters/setters since you no longer have to "control" how the data is accessed.

The second thing I did, which I believe Bob also did, is I noticed the duplication and abstracted that out into a function. The whole purpose of a function is to get rid of duplicated logic. This helps reduce the code density and fat-finger copy/paste errors. I will say, I'm glad the original authors duplicated the code first, because it makes it _very_ easy to see the duplicated logic and refactor the code. It's much better to duplicate your code first and then abstract later, than it is to abstract in advance and end up with functions that only have one reference everywhere for the sake of "readability".

> "Duplication is far cheaper than the wrong abstraction" - [Sandi Metz](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction)

Once I abstracted the duplicated code, the intent of the function became much clearer.

The last step I took was inlining the if-statements to reduce the level of nested logic. I agree with Bob here as well, it's much easier to follow the control flow of a function if there is less nesting. However, this is just a preference, and leaving the if-statements nested is perfectly valid. I also cached the `isTestPage` variable so that it's clear what that line of code is doing and it makes it easier to change this code in the future (I just have to modify the single line instead of copying the change in several places).

Notice, the one thing I did _not_ do is remove the logical flow of this function and add unnecessary side effects. I left all the core logic in the function and the reader can still follow the control flow. Additionally, the function that I added does not mutate the data (any more than it already was) but returns a new String with the result. This allows me to re-use that function with certainty that it won't add side-effects that the reader now has to look for.

Bob's code on the other hand relies on side-effects and removes the core flow of logic. This requires the reader of the function to either implicitly be aware of that fact, or to risk modifying code and then cursing at themself later when they find out changing that function accidentally changed 500 unrelated pieces of state unintentionally.

After this, Bob goes through the qualities of a good function. I actually agree with a lot of what he says, but some of it is nonsense, and he doesn't appear to follow his own advice (except for the biggest piece of nonsense which is functions should be small).

### Small!

His first piece of advice is functions should be small, and then they should be _smaller_. This is nonsense. Balderdash. Hogwash. Rubbish. And any other way you can think of saying, "it makes no sense".

Functions should be concise, and they should reduce duplication. Sometimes, a piece of logic is complex and large, and it doesn't matter if you break up the logic into a bunch of tiny pieces of "beautiful" steaming piles of garbage, the logic is still there, it's just hidden now between a million jumps!

John Carmack has an old article on this that I think summarizes why the idea of forcing functions to be small is drivel better than I ever could, you can read that [article here](http://number-none.com/blow/blog/programming/2014/09/26/carmack-on-inlined-code.html) (and I highly recommend it).

So, my piece of advice would be to ignore this. Instead, functions should reduce duplication, period. If you have a function that is only ever called in one other place, then you should inline it so the logic is all in one cohesive place. This helps the reader, since they no longer have to jump around the code unnecessarily, and it helps the maintainer, since they can more easily see the logic and reduce it, simplify it, or fix it.

### Do One Thing

Next, Bob says a function should only do one thing. This is great, however it's not feasible. For example, most functions have an entry point called `main`. Does that function do one thing? Technically yes, it runs the program. But is it really one thing? Most people would say, no it's doing a whole lot of things.

Bob kind of covers this as well by mentioning that it's really hard to determine what _one thing_ is. He says you can tell if a function is doing more than one thing if you can abstract another function out of it. This is more bologna. The purpose of a function is to _reduce duplication_ and abstract _repeated logic_ into a cohesive unit. Don't abstract random lines of code for no reason, abstract them because they're used in multiple places!

He ends this section by saying that if you have sections within your functions, then it's doing more than one thing. So what? Modern IDEs can collapse sections and have been able to collapse sections for a _long time_ ([this](https://stackoverflow.com/questions/2828065/how-do-i-expand-all-collapsed-code-regions-in-vs2005) stack overflow question shows that even VS2005 supported code collapsing, so this feature has been around for at least 18 years at the time of this writing, and _Clean Code_ was released in 2008, so it was clearly available then too!).

Further, his next rule says that code should read like a narrative. Well, narratives contain sections do they not? If we're following his logic about the no "sections" rule and "keep it small" rule, then if you were writing a book, it should only contain the main heading and paragraph. Then you can have footnotes on separate pages that you have to flip to in order to read all the subheadings with their paragraphs.

Books (well-written ones at least) are not written this way. Why? Because no reader is going to waste time flipping back and forth a million times to read your dumb _narrative_. His own book even belies this fact! He has heading, and subheadings. These sections help improve the readability of his book, and they help improve the readability of your code.

So, sections in your code is a good thing. It helps simplify your functions and makes sure all the logic pertaining to that function lives in one place.

### One Level of Abstraction per Function

He then says that one piece of code shouldn't handle multiple levels of abstraction. I can't get behind this. Once upon a time, for-loops and if-statements did not exist. They were coded by a certain pattern of jump/goto instructions in assembly. Nowadays, we use structured programming which embeds these concepts as abstract parts of the programming language itself. Whenever you use a loop or an if statement, you are mixing a higher level of abstraction with whatever your code is doing, and it's a _good thing_.

Mixing levels of abstraction is how we simplify programs. There's nothing wrong with it and I have trouble thinking of a reason where mixing levels of abstraction actively makes your code worse. You can't add assembly into your C++ code without mixing those levels of abstraction _somewhere_. I'd rather have the mixing be very obvious and in plain sight, rather then hidden away under multiple layers of cruft that do nothing but obfuscate that fact.

### Switch Statements

Honestly, I have no words. This whole section explains that switch statements are bad and should be hidden behind polymorphism or `ABSTRACT FACTORY`s or something. He literally says, and I quote "The solution to this problem is to bury the `switch` statement in the basement of an `ABSTRACT FACTORY` and never let anyone see it" (Clean Code, P.38). Wow... The purpose of writing a readable program is to make your intentions clear.

Bob is actively promoting the idea that you should _obfuscate your intentions_ from future readers of the program. This statement alone is probably one of the worst statements I've seen so far in the book.

My suggestion, use a switch statement if it makes sense, and preferabbly make sure your compiler emits warnings for unhandled switch cases.

### Use Descriptive Names

I _almost_ agree with this whole section. The one thing I disagree with is when he says "Don't be afraid to make a name long". Actually, be afraid of making overly verbose names. Overly verbose naming schemes only increase the noise-to-signal ratio in your code.

Ideally, the name should be descriptive and short. Sometimes the name has to be a bit longer, and that's ok. If they're too long, as I'm reading your program my mind will silently truncate the long and verbose names and skip over pieces of code that are potentially important. In the same way, if I make an overly long sentence as part of an overly long paragraph, your mind will begin to wander and lose focus. Also, you may lose track of where you are in this paragraph because it becomes too much information density for your brain to easily parse. I bet at this point, you are probably scrolling faster to get past this monstrous paragraph because it is unnecessary and adds mental "noise" which is very obtrusive to trying to read this article. In addition, you are probably getting annoyed from the mental burden I am placing on you. You feel like this paragraph may contain some useful insight, but at this point, it absolutely contains no other insight than to show you how painful it is when things are described in an overly verbose manner.

Sorry about that, I guess... a thousand words is worth a thousand words?

This is one of the reccurring themes in the book I think Bob is confused about. He seems to correctly see the problem, dense blocks of code that have very low signal-to-noise ratios, and then misunderstands what he's seeing and proposes splitting up that information density in a horrible way.

For example, he thinks functions should be small because it reduces the density of information. While this is true, sometimes logic is dense and that density should be visible. In those cases, you should use liberal whitespace and comments explaining _why_ the code is doing what it's doing.

Java is particularly bad for increasing information density by promting styles like brackets on the same line of function declarations. This probably makes a lot of Java code seem more dense than it really is and further adds to this confusion.

### Function Arguments

I'm going to kind of skip over most of the nonsense in this bit. Bob argues that functions should have few arguments, and ideally none. Then he argues that functions should have no side-effects. It's impossible to have a function with 0 arguments that does not have any side-effects, so his two pieces of advice are in conflict with each other.

Think about it, if your function has 0 arguments, it can only be operating on data that is defined externally. This, by definition, is a function with side-effects. (Yes, I guess if you have a function with 0 arguments that returns a reference to an immutable data structure, it's technically side effect free. However this sounds eerily like a getter, and if you're already marking data as mutable/immutable properly, getters are unnecessary!)

The reason Bob gives this advice is probably because he seems to mistakenly believe that if you mutate the object that the method lives in, then this is not _really_ a side effect. He writes later in the next section "If your function must change the state of something, have it change the state of the owning object" (_Clean Code_ P. 45).

I agree that functions shouldn't have too many arguments, but sometimes it's unavoidable and that's ok. If you do have to add several arguments, don't create a _Data Transfer Object_ or some similar nonsense (unless that object is actually being used for additional processing and represents something more than an object for the sole purpose of reducing parameters. Or, you have profiled and found out that too many parameters is causing increased latency and using a DTO fixes that).

Instead, write clear documentation about the purpose of all the arguments. I think the Win32 documentation is actually a great example of this. For example, [this function](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-createwindowa) `CreateWindowA` takes 7 arguments. Every single one of those arguments is useful in certain scenarios, and every single one of those arguments is well explained in the documentation.

You could reduce some of those arguments by doing something like having a `Vec2{x, y}` structure instead of separating those. However, that isn't necessary and this code is just as readable as code that wraps the parameters in structures like that.

Another important note that Bob has in here is flag arguments. I agree with this. You should not pass boolean's as flags to a function. It makes reading the code very difficult (for example `createWindow(true)` vs `createWindow(WindowFlag::Maximized)`). I disagree with his solution though, which is to split the function. Sometimes that's the right call, but other times it's fine to create an enum and strongly type the flag. This makes it clearer for the reader, and easier to extend if that flag ever needs to have more than 2 possible values (true, false).

### Have no Side Effects

I agree with this section. Your functions should have as few side effects as possible. Additionally, if your language supports marking arguments/data as mutable/immutable, use it. Mark your stuff as `const` to make it clear to future readers/users/maintainers what that function expects of the data being passed to it.

### Prefer Exceptions to Returning Error Codes

I think this was a novel idea and that it had some merit in 2008. However it's 2023 now, and the results of the `try {} catch {}` pattern are in, and it doesn't look good. From experience, and reading several other codebases, the most common use case of try/catch blocks is to ignore error handling altogether. Additionally, try/catch blocks promote goto statements in the form of jumping out of deeply nested call stacks to some random place in the code. Both of these reasons are reasons I _prefer_ error codes to try/catch blocks.

If you use error codes, you force the developer to handle the error as soon as it occurrs. Further, Bob's example of why this is bad is just silly. He says error codes are bad because then you end up with something like this:

```java
if (deletePage(page) == E_OK) {
    if (registry.deleteReference(page.name) == E_OK) {
        if (configKeys.deleteKey(page.name.makeKey()) == E_OK) {
            logger.log("page deleted.");
        } else {
            logger.log("configKey not deleted.");
        }
    } else {
        logger.log("deleteReference from registry failed.");
    }
} else {
    logger.log("delete failed.");
    return E_ERROR;
}
```

Bob's proposed fix is to do something like this:

```java
try {
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey());
} catch (Exception e) {
    logger.log(e.getMessage());
}
```

This proposed fix has a couple problems:

1. It's unclear where this function can fail
2. It's unclear what failure states may result from this function
3. It's unclear what should happen if this function fails (the caller has no idea where the function failed and doesn't know what to do because there's no error returned)

My proposed fix would be this:

```java
if (deletePage(page) != E_OK) {
    logger.log("delete failed.");
    return E_ERROR;
}

if (registry.deleteReference(page.name) != E_OK) {
    logger.log("deleteReference from registry failed.");
    return E_ERROR;
}
if (configKeys.deleteKey(page.name.makeKey()) != E_OK) {
    logger.log("configKey not deleted.");
    return E_ERROR;
}

logger.log("page deleted.");
return E_SUCCESS;
```

Wow, magical isn't it? I would like to give Bob the benefit of the doubt and assume he doesn't refactor the code like this because he's abiding by the outdated notion that a function should only ever have one return statement...

Except, later on, he mentions that "if you keep your functions small, then the occasional multiple `return`, `break`, or `continue` statement does no harm and can sometimes even be more expressive than the single-entry, single-exit rule" (_Clean Code_ P.49). I agree with this statement, and we can apply it to this error handling.

Now, this function is up front about the failure conditions. Each failure condition is listed first in the function. Now future readers/maintainers can clearly see the assumptions this function makes and where it can fail. Additionally, you can make each error code more explicit so that the caller can respond to and handle these errors themself.

Additionally, the errors are actually handled here, or the handling is forwarded up the call stack. In Bob's version, the errors are hidden and suppressed and the caller can just ignore them.

You could go one step further though, and if your language supports multiple return values you could follow a pattern of returning a value + an error code. [Here's an article](https://www.gingerbill.org/article/2018/09/05/exceptions-and-why-odin-will-never-have-them/) about the Odin language defending this use case if you're curious.

### Functions Conclusion

Bob ends this chapter by showing his final "cleaned" version of the original FitNesse function we started with following all the rules he created. The monstrosity looks like this (see how much you can understand about this in 3 minutes!):

```java
public class SetupTeardownIncluder {
    private PageData pageData;
    private boolean isSuite;
    private WikiPage testPage;
    private StringBuffer newPageContent;
    private PageCrawler pageCrawler;

    public static String render(PageData pageData) throws Exception {
        return render(pageData, false);
    }

    public static String render(PageData pageData, boolean isSuite)
        throws Exception {
        return new SetupTeardownIncluder(pageData).render(isSuite);
    }

    private SetupTeardownIncluder(PageData pageData) {
        this.pageData = pageData;
        testPage = pageData.getWikiPage();
        pageCrawler = testPage.getPageCrawler();
        newPageContent = new StringBuffer();
    }

    private String render(boolean isSuite) throws Exception {
        this.isSuite = isSuite;
        if (isTestPage()) 
            includeSetupAndTeardownPages();
        return pageData.getHtml();
    }

    private boolean isTestPage() throws Exception {
        return pageData.hasAttribute("Test");
    }

    private void includeSetupAndTeardownPages() throws Exception {
        includeSetupPages();
        includePageContent();
        includeTeardownPages();
        updatePageContent();
    }

    private void includeSetupPages() throws Exception {
        if (isSuite)
            includeSuiteSetupPage();
        includSetupPage();
    }

    private void includeSuiteSetupPage() throws Exception {
        include(SuiteResponder.SUITE_SETUP_NAME, "-setup");
    }

    private void includeSetupPage() throws Exception {
        include("SetUp", "-setup");
    }

    private void includePageContent() throws Exception {
        newPageContent.append(pageData.getContent());
    }

    private void includeTeardownPages() throws Exception {
        includeTeardownPage();
        if (isSuite) 
            includeSuitTeardownPages();
    }

    private void includeTeardownPage() throws Exception {
        include("TearDown", "-teardown");
    }

    private void includeSuiteTeardownPage() throws Exception {
        include(SuiteResponder.SUITE_TEARDOWN_NAMAE, "-teardown");
    }

    private void updatePageContent() throws Exception {
        pageData.setContent(newPageContent.toString());
    }

    private void include(String pageName, String arg) throws Exception {
        WikiPage inheritedPage = findInheritedPage(pageName);
        if (inheritedPage != null) {
            String pagePathName = getPathNameForPage(inheritedPage);
            buildIncludeDirective(pagePathName, arg);
        }
    }

    private WiiPage findInheritedPage(String pathName) throws Exception {
        return PageCrawlerImpl.getInheritedPage(pageName, testPage);
    }

    private String getPathNameForPage(WikiPage page) throws Exception {
        WikiPagePath pagePath = pageCrawler.getFullPath(page);
        return PathParser.render(pagePath);
    }

    private void buildIncludeDirective(String pagePathName, String arg) {
        newPageContent
            .append("\n!include ")
            .append(arg)
            .append(" .")
            .append(pagePathName)
            .append("\n");
    }
}
```

Please excuse me while I rest my hands after the arthritis induced from writing out that behemoth... My God, can anyone sit with a straight face and tell you that _this_ is _more readable_ than the original implementation?! This, more than anything, hammers down the fact that "clean code" is a bombastic, mythical ideology put forth by people living high above the clouds:

> “...one sure tip-off to the fact that you’re being assaulted by an Architecture Astronaut: the incredible amount of bombast; the heroic, utopian grandiloquence; the boastfulness; the complete lack of reality. And people buy it! The business press goes wild!” - [Joel Spolsky](https://www.joelonsoftware.com/2001/04/21/dont-let-architecture-astronauts-scare-you/)

And, just for comparison's sake, here's what I changed the code to. I guess you can decide for yourself which version is more readable:

```java
public static String getPageData(String pageName, WikiPage wikiPage) {
    StringBuffer result = new StringBuffer();
    WikiPage setup = PageCrawlerImpl.getInheritedPage(pageName, wikiPage);

    if (setup != null) {
        WikiPagePath pagePath = setup.pageCrawler.getFullPath(setup);
        String pagePathName = PathParser.render(pagePath);
        result.append("!include -setup .")
            .append(pagePathName)
            .append("\n");
    }
    
    return result.toString();
}

public static String testableHtml(
    PageData pageData, 
    boolean includeSuiteSetup
) throws Exception {
    WikiPage wikiPage = pageData.wikiPage;
    StringBuffer buffer = new StringBuffer();

    boolean isTestPage = pageData.hasAttribute("Test");
    if (isTestPage && includeSuiteSetup) {
        String suiteSetupData = getPageData(SuiteResponder.SUITE_SETUP_NAME, wikiPage);
        buffer.append(suiteSetupData);
    }

    if (isTestPage) {
        String setupData = getPageData("SetUp", wikiPage);
        buffer.append(setupData);
    }

    buffer.append(pageData.content);

    if (isTestPage) {
        String teardownData = getPageData("TearDown", wikiPage);
        buffer.append(teardownData);
    }

    if (isTestPage && includeSuiteSetup) {
        String suiteTeardownData = getPageData(SuiteResponder.SUITE_TEARDOWN_NAME, wikiPage);
        buffer.append(suiteTeardownData);
    }

    pageData.setContent(buffer.toString());
    return pageData.getHtml();
}
```

## Chapter 4: Comments

Most of what Bob covers in this chapter is pretty on point. The best rule of thumb I've found is a comment should explain _why_ a piece of code is doing what it's doing, not _what_ the piece of code is doing. Following that rule of thumb pretty much covers all of Bob's sub-sections.

The only things I would update in this chapter are: the section about legal comments, position markers, and too much information.

The legal comments section is outdated, you should have a license in your repository that covers all the code. Leaving a legal header at the top of each file is unnecessary and visual noise and I wouldn't recommend it (unless you're explicitly mandated by your company to do it).

Comments that are position markers like: `// **** Private functions ****` are useful in my opinion. They can be helpful to grep through the codebase or file and look for a specific section.

Lastly, Bob has a section explaining that some comments provide too much information by paraphrasing the spec it's following etc. I agree with him to a point on this. Your code shouldn't include the entire spec that it's implementing, that's verbose and redundant. However, I've been watching [Andreas Kling](https://www.youtube.com/c/andreaskling) on YouTube lately, and he will often paste the steps of the spec that the piece of code is implementing with a link to the original URL. This is supremely helpful in clarifying the intent of the code and linking to the source and I think this is a perfectly valid use of comments.

One additional thing I would add that Bob didn't, is I believe comments that link back to the source material are great. For example, if you found an answer on Stack Overflow and adapted your code to match that, leave a link because it will be useful to future you!

## Chapter 5: Formatting

I skimmed through this chapter very briefly. For all intents and purposes, this is irrelvant today. Use a code linter/formatter for code formatting. 'nough said.

## Chapter 6: Objects and Data Structures

This chapter starts off with a couple of code listings:

```java
// Listing 6-1
public class Point {
    public double x;
    public double y;
}

// Listing 6-2
public interface Point {
    double getX();
    double getY();
    void setCartesian(double x, double y);
    double getR();
    double getTheta();
    void setPolar(double r, double theta);
}
```

Bob immediately pontificates on how beautiful Listing 6-2 is and how it abstracts all the details away and the user never has to care. This, in essence, is the main problem with OOP. Have you ever tried to understand a codebase littered with interfaces hiding all the abstractions? I have. Usually you'll encounter something like this:

```java
public void getOrders(ILogger logger, int numOrders, IResponse response) {
    // Do some stuff
    response.setStatus(404);
    // Let's Ctrl+Click (or Cmd+Click for you mac users) on 
    // IResponse to go to the implementation to understand what's
    // happening
}
```

And now the madness ensues. When you try to jump to the implementation of `IResponse` instead of being sent to the actual implementation being used at the moment, you are sent to the interface. Why is that? Because depending on where the function is called, `IResponse` could be an `HttpResponse`, or a `SoapResponse`, or a `JsonHttpResponse`, or who the heck knows. In reality, it's most likely one of those things and only ever one of those things (at least more than 90% of the time when I'm debugging a corporate codebase, there's only ever one implementation).

What's wrong with this? Well now, you as a maintainer have no freaking clue what's actually happening or what's being used or what to do about it. Interfaces are a tool. Ideally, they should be used when you're trying to provide an interface to some library user. That library user can implement the interface, and their code will just work. The library maintainer doesn't need to know the users code, and the user doesn't need to know the maintainers code, they just need to know which functions to implement.

Instead, what Bob is recommending here, and what usually plays out in reality, is *every single class is an interface* (to be fair, he doesn't explicitly say that here, but a lot of his other writing and ideas suggest this). This makes debugging impossible, understandability impossible, and builds a massive web of ever increasing complexity. How would I restructure this? How about:

```java
// Listing 6-MyImplementation

// The majority of my use cases are cartesian, so I'll default to 
// cartesian coordinates. If your codebase is different, then you
// can swap the default
public class Point {
    public double x;
    public double y;
}

public class CartesianPoint {
    public double r;
    public double theta;
}
```

The beautiful thing about listing 6-MyImplementation is that now nobody is confused! The type declares very loudly exactly what you're working with. There is no obfuscation through interface-ification. Everything is immediately visible and the user doesn't have to worry what the hell they're getting passed everytime they see something like `public void foo(Point whoKnowsWhatThisIs)`.

After this, Bob goes on to explain the difference between data structures and objects. He seems to be referring to data structures like POD (Plain Old Data) in C++: `struct`'s that have dumb data. As opposed to the typical definition programmers use data structures for: a common structure/container of generic data like `HashMap`s, `List`s, `Vector`s, etc.

I think his explanation here is actually quite good, and he gives a good qualification at the end of this section:

> Mature Programmers know that the idea that everything is an object *is a myth*. Sometimes you really *do* want simple data structures with procedures operating on them. (Clean Code P.97)

This is good advice, and one that I don't see a lot of the actual examples in this book following.

This chapter ends with advice about using DTOs (Data Transfer Objects) to transform dumb data into an object. This is dumb advice, and it accomplishes nothing but adding bloat to your code. Think about it, why do we need dumb objects and smart objects in the first place? Well, according to most of the advice in this book, it's so that functionality can be coupled with data. This leads to weird concepts like DTOs. Because now, instead of having standalone functions to operate on data, you have to somehow convert that data to a "Smart" object.

What do you accomplish through this? Well... nothing really. Some people will argue that binding data to logic avoids leaking implementation details. Well, if you don't want to leak implementation details, make sure the data structures are strictly defined internally to your library. And if you're coding for yourself, then why the hell would you hide implementation details from yourself?

You, as the maintainer of the code, need to know how it works! Why would you ever obfuscate that from yourself? All you're doing is making it harder to maintain and shooting yourself in the foot for no reason! So, instead of transforming dumb data to "smart" data, how 'bout you consider using some standalone functions instead?

```cpp
struct SvgObject {
    std::vector<Path> paths;
    Vec4 fillColor;
    PathFillRule fillRule;
}

namespace Svg {
    SvgObject copySvg(const SvgObject& svg);

    SvgObject createSvg();
    void lineTo(SvgObject& svg);
    void moveTo(SvgObject& svg);
    void close(SvgObject& svg);
}
```

This little mock implementation of a hypothetical SVG object makes sure the data is dumb and easily accessible, but the programmer has free access to an API that allows them to do several things with it. No "smart" data here, but you still get all the benefits of the "smartness"!

This blog post is already very long, so I'm going to cut it off here. If I decide to jump back into this holy relic among corporate programmers far and wide, I'll write a second blog post with a review of what I read. Hopefully you enjoyed this, if you don't agree with what I said, cool. Coding is very opinionated and most of the time there is no "right" answer, so you have every right to your opinion.

Anyways, see ya.
