# Mindjoy Coding Challenge

Hi!

Your job is to build a class scheduler, which organizes kids into
different classes. Each class consists of about 6 kids (although it
can be more or less), and each class runs at a particular time
(e.g. Tuesday at 17:00).

There are two goals:

1. The schedule must be correct
2. The schedule should try to be as optimal as possible. We'll present
   the "optimal" conditions later. Bear in mind that _the optimum_ solution
   is probably an NP-hard problem, so don't worry about trying to find
   the single best solution. So long as your program attempts to find
   a reasonably good solution, in a reasonable amount of time, that's fine.

### Example Input

```json
{
  "students": [
    {
      "age": 9.7,
      "skill": 3
    },
    {
      "age": 10.9,
      "skill": 2
    }
  ],
  "timeslots": [
    {
      "day": "Monday",
      "time": "17:00",
      "studentsAvailable": [0, 1]
    },
    {
      "day": "Monday",
      "time": "18:15",
      "studentsAvailable": [1]
    }
  ]
}
```

### Example Output

```
Class 0 (Monday 17:00)
	Student-0   9.7  3
	Student-1  10.9  2
```

### Explanation

The example input is a trivial setup, but it will help to illustrate the problem.

There are only two kids, and two timeslots.
If we look at the `17:00` timeslot, in the `studentsAvailable` field we see `[0,1]`. The
0 and 1 are indices into the `students` list. This list of `[0,1]` tells us that both
students are available on Mondays at `17:00`.

If we look at the other timeslot (at `18:15`), we see that only student 1 is available
then.

There are two potential solutions to this input. The first possible solution is to
create two classes. One at `17:00`, and another at `18:15`, and place kid 0 into
the first class, and kid 1 into the second class. This is a silly solution, because
we _want_ kids to be together.

The better solution is to create a single class at `17:00`, and place both kids in
that class. This is what the `Example output` shows.

### Correct Solutions

A correct solution is one in which no kid is placed in a timeslot for which they
are unavailable. This is the only criteria for correctness.

### Optimal Solutions

An optimal solution is one which minimizes what we call a _loss function_. The term
_loss function_ comes from the Machine Learning world, but if you've never heard of
it before, don't be frightened. It's just a number which we are trying to minimize.
You may also have heard of the term _objective function_, and it's the same thing.

The loss function for our schedule is defined as:

`total_loss = average of individual loss values of each class`

In other words, we are trying to minimize the loss function for each class that
we create. This doesn't mean that a solution which produces 5 classes is better
than a solution which produces 6 classes. We are trying to minimize the average.

The loss function for each class is (in broad terms, ignoring some details):

`class_loss = [variance of kid ages] + [variance of kid coding ability] + [class size]`

If we add the details in, we get

`class_loss = 0.6 * [variance of kid ages] + 0.4 * [variance of kid coding ability] + 0.5 * abs(6 - class_size)`

Let's explain the above class loss equation:

- _0.6 \* Variance of kid ages_: The variance of the kid's ages is simply the
  `standard deviation` of the set of kid's ages, measured in years. We use
  fractional years (eg 9.5 means 9 years and 6 months old).
  The 0.6 is simply a weighting constant.
- _0.4 \* Variance of kid coding ability_: The variance of the coding ability is simply
  the `standard deviation` of the set of kid's coding ability scores.
  The 0.4 is simply a weighting constant.
- _0.5 \* abs(6 - classSize)_: What we're doing here is trying to make sure that
  each class is as close as possible to 6 kids.
  The 0.5 is simply a weighting constant.

### Rules

- There can be multiple classes at one timeslot. For example, if there are 10 kids who are all available
  at `Monday 17:00`, then it might be best to create two classes for that timeslot, and place 5
  kids into each of them.
- You may use any language you want.
- You may not use an optimization library. Just use the standard library for your language of choice.
- Your program may not take more than 5 seconds.
- Your program only needs to work on the provided `input.json` file.
- If there is anything confusing about the task, do not hesitate to reach out and ask for clarity.
  Any questions you might have about ambiguities are probably a good sign!

### Hints

- A monte carlo solution is fine. You don't need to be fancy to find reasonably good solutions.
- In other words, it's fine if your program tries a lot of random solutions, or at least if it starts out by doing that.
