# Versatile question bank

The Spark question bank is built on the **QuML (Question Markup Language) specification**, an open standard for representing questions and question sets.

#### Question types supported

* Multiple choice, with single or multiple correct answers
* Fill in the blank
* Match the following
* Subjective or free-text responses

#### Question set features

* Randomised question ordering
* Timed assessments with configurable limits
* Hint support per question
* Configurable scoring and pass or fail thresholds
* Section-based question sets for grouping questions into themed sections

#### Use cases

The same question bank infrastructure supports multiple use cases. The difference is in configuration, not code.

| Use case              | Configuration                                            |
| --------------------- | -------------------------------------------------------- |
| Practice exercise     | No timer, hints enabled, instant feedback                |
| Graded assessment     | Timed, no hints, score-based pass or fail                |
| Survey                | No correct answers, all questions required               |
| Observation checklist | Structured form-like question set for field observations |
| Quiz                  | Short, timed, gamified scoring                           |

#### QuestionSet Player and Editor

Both the QuestionSet Player and QuestionSet Editor are V2 web components. Each can be embedded independently in any web application, like the content players.
