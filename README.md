# Questions I ask at interviews

## JavaScript

#### Promises
* How run an array promises one after another? 
  ~~~~js 
  function promiseCreator(timeout) {
    return prevResult =>
      new Promise(resolve =>
        setTimeout(
          () => void (console.log(`${timeout} after ${prevResult}`), resolve(timeout)),
          timeout
        )
      );
  }
    
  const chainPromises = [promiseCreator(300), promiseCreator(3000), promiseCreator(2000)];
    
  //like so
  chainPromises
    .reduce((chain, promise) => chain.then(promise), Promise.resolve(0))
    .then(() => console.log('end'))
      
  //or
  (async () => {
    let prevRes = 0;
    for (const promise of chainPromises) prevRes = await promise(prevRes)
    console.log('end')
  })()  
  ~~~~ 
* `promise.finally(() => { })`  
  **Run order:** 't0' -> 'f0' -> 'falback' -> 'f1' -> 't2'  
  because 'finally' always run and when we have an error it's run before 'catch'
  
  **Final result:** 'failback'  
  unlike 'try...catch...finally' return from 'finally' does not affect anything, so last 'then' get value from last successful promise (i.e, from 'catch')
  ~~~~js 
  new Promise(resolve => setTimeout(() => resolve('+')), 1000)
    .then(() => { console.log('t0'); throw new Error(0) })
    .then(() => 't1')
    .finally(() => console.log('f0'))
    .catch(() => { console.log('falback'); return 'falback' })
    .finally(() => { console.log('f1'); return 'f1' })
    .then(res => { console.log('t2'); console.log(`=> ${res}`) })
  ~~~~

#### Bikes creating
* [Implement currying](https://codepen.io/LyulyaevMaxim/pen/YmVqLw): `curry(callback)(1)(2,3)(4)`                          
  need work with callbacks which can have limited as and unlimited quantity arguments