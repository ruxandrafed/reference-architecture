# Unit testing

## Why

When designing or implementing a feature, we want to know that we are doing it properly. Once the feature is implemented, we want to ensure that it keeps working.

## What

As part of our [Continuous Integration](../process/continuous-integration.md) practices, we are pushing for Test Driven Development, where unit tests are written BEFORE a new feature. It falls in line with the construction proverb: "measure twice, cut once".

By writing the test first:

- Ensures the code is loosely coupled
- Your code evolves in small steps
- You get automated regression testing
- You get living documentation as to how the system works
- etc.

## How

- Start with your test first, to ensure that the test fails
- Implement the feature that makes the test pass
- Avoid [fragile tests](https://www.youtube.com/watch?v=URSWYvyc42M) and tight coupling


## When

- Writing:
 - As stated in `How`, we are recommended to always write unit tests first before functional code, therefore following the [Test Driven Development][TDD] practice

- Running:
 - Unit tests are recommended to be executed as part of the pre-commit hook
 - Unit tests are required to be executed as part of the delivery pipeline

## Standards

### 1: Coverage as a %

We DO NOT only look at code coverage itself as a %, because enforcing an 100% code coverage can easily lead to "happy path" unit tests and potentially worse code quality. [Don't be fooled by 100% code coverage][Fooled by 100%] has a great explanation on this topic.

Additionally, we look at branch coverage instead of simply line coverage. Because we need to examine that each path of the application's control structure (e.g: if statements) has been executed and covered.

We understand that diminishing return kicks in at higher coverage. The return of investment to increase the coverage % from 98 to 99, is going to small and  at times unjustifiable.

That said, we cannot 
- Just let our coverage stay stale at 50% for months and not improving
- Cheat the % by writing test such as ```assertTrue(divide(10, 2) != 0)```

Given the above, we have two standards regarding code coverage

1: Make sure the applications' branch coverage is consistently improving until it's stable at or above 90%. If the application's out of the box unit test coverage is below 80%, then it should at least achieve 80% mark within 2 months time. Once at 80%, it should achieve 90% within another 2 months time.

2: To improve the quality of our unit tests, we should leverage [mutation testing][Mutation testing] to validate that they are in fact safeguarding our functionalities. Mutation tests essentially modifies the application code (e.g: flipping operator, or removing an entire code block) to ensure the unit tests fail (as they should).

For Javascript/Typescript applications (that we have), [Stryker][Stryker] is a great tool for mutation testing. We recommend that the testers and developers on each application work together to set it up, run periodic audits (say nightly, as the audits can take some time to run, we do not recommend embedding it in the delivery pipeline), identify potential gaps in the unit tests, so as to refine and improve them.


### 2: Code quality metrics

We use [Sonarqube][Sonarqube] for static code analysis purposes, to meet the above coverage standards. 

For details on what value it adds to reference architecture and how to have it set up for your project, check out [Sonarqube in TELUS Digital][Sonarqube in TELUS Digital]

### 3: Style

For testing React higher order components, it is preferred to keep the presentational component and the enhancing container in separate source files, then wrap in `index.js` to export the enhanced component. This way, testing can be done separately on the presentational component, and then the higher order component, without adding in extra named exports.

ie: Given a `Card` that gets wrapped in an HoC, the directory structure should look as follows:
```
- Card/
 - __tests__/
   - Card.spec.jsx
   - EnhancedCard.spec.jsx
 - Card.jsx
 - EnhancedCard.jsx
 - index.js
```

## Who

@delivery @dev

## References

- [Magic tricks of unit testing](https://www.youtube.com/watch?v=URSWYvyc42M) _Ruby, but excellent concepts_
- [Don't be fooled by 100% Code Coverage][Fooled by 100%]


[Starter-kit: unit test]: https://github.com/telusdigital/telus-isomorphic-starter-kit/blob/master/DOCKER.md#unit-testing
[TDD]: https://en.wikipedia.org/wiki/Test-driven_development
[Sonarqube]: https://github.com/SonarSource/sonarqube
[Sonarqube in TELUS Digital]: https://github.com/telusdigital/sonarqube
[Fooled by 100%]: http://ordepdev.me/posts/code-coverage
[Mutation testing]: https://en.wikipedia.org/wiki/Mutation_testing
[Stryker]: http://stryker-mutator.io/

