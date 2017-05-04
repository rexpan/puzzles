# Break Lock

[BreakLock](https://maxwellito.github.io/breaklock/)

## Solution

```js
;((() => { 

// for each [m, a, b], you cannot connect a--b if have not connect m
const notRules = [
    [2, 1, 3],
    [5, 4, 6],
    [8, 7, 9],
    [4, 1, 7],
    [5, 2, 8],
    [6, 3, 9],
    [5, 1, 9],
    [5, 3, 7],
];

const digit = [1,2,3,
               4,5,6,
               7,8,9];

// Fill the hints in here
const hints = [
    { guess: [1, 2, 3, 4, 5, 6], correct_wellPlace:0, correct_wrongPlace:5 },
    { guess: [4, 1, 2, 5, 6, 3], correct_wellPlace:0, correct_wrongPlace:5 },
    { guess: [5, 6, 7, 3, 2, 1], correct_wellPlace:1, correct_wrongPlace:3 },
    { guess: [2, 5, 4, 8, 3, 1], correct_wellPlace:1, correct_wrongPlace:4 },
    { guess: [3, 6, 5, 8, 1, 4], correct_wellPlace:2, correct_wrongPlace:4 },
];
const cs = createCandidate(digit, 6);

const solutions = hints.reduce((cs, hint) => cs.filter(c => isMatch(c, hint)), cs);

console.clear();
console.table(solutions);
console.log(solutions[Math.floor(Math.random()*solutions.length)]);



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

// Create list of candidates [1,2,3] ... [9,8,7]
function createCandidate(symbols, n) {
    if (n == 1) return symbols.map(x => [x]);

    const xss = createCandidate(symbols, n-1);
    const yss = [].concat(...(xss.map(xs => symbols
        .filter(x => !xs.includes(x))
        .map(x => [...xs, x]))));

    return yss.filter(ys => 
        notRules.every(([x,y,z]) => {
            [x,y,z] = [x,y,z].map(x => ys.indexOf(x));

            if ((y < 0) || (z < 0)) return true;
            if (x < 0) return (Math.abs(y-z) > 1);
            if (Math.abs(y-z) > 1) return true;
            return x < y && x < z;
        })
    );
}

})());
```
