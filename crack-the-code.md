# Problem
 
![Crack the code](https://img-9gag-fun.9cache.com/photo/agL1n56_700b.jpg)

([source](https://9gag.com/gag/agL1n56))

> A numeric lock has a 3 digit key
> - `6 8 2` One number is correct and well placed
> - `6 1 4` One number is correct but wrong placed
> - `2 0 6` Two numbers are correct but wrong placed
> - `7 3 8` Nothing is correct
> - `7 8 0` One number is correct and well placed


# Solution

```js
;((() => { 

const hints = [
    { guess: [6,8,2], correct_wellPlace:1, correct_wrongPlace:0 },
    { guess: [6,1,4], correct_wellPlace:0, correct_wrongPlace:1 },
    { guess: [2,0,6], correct_wellPlace:0, correct_wrongPlace:2 },
    { guess: [7,3,8], correct_wellPlace:0, correct_wrongPlace:0 },
    { guess: [7,8,0], correct_wellPlace:0, correct_wrongPlace:1 },
];

const digit = [0,1,2,3,4,5,6,7,8,9];

// Candidates [0, 0, 0] ... [9, 9, 9]
const cs = [].concat(...digit.map(x => [].concat(...digit.map(y => digit.map(z => [x,y,z])))));

const solutions = hints.reduce((cs, hint) => cs.filter(c => isMatch(c, hint)), cs);

return solutions;

function check(guess, truth) {
    const correct_wellPlace = guess.filter((x,i) => x == truth[i]).length;
    const correct_wrongPlace = guess.filter((x,i) => x != truth[i] && truth.includes(x)).length;
    return { correct_wellPlace, correct_wrongPlace };
}

function isMatch(truth, {guess, correct_wellPlace, correct_wrongPlace}) {
    const r = check(guess, truth);
    return (
        r.correct_wellPlace  == correct_wellPlace && 
        r.correct_wrongPlace == correct_wrongPlace 
    );
}



})());
```

```
0 4 2
```
