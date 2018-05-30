# truth-runner
> This project will deliver a CLI that watches a well-formed SPEC (we call it a **single source of truth**) and uses handmade translating and transpiling services to scaffold, build, test and document a complete Vue.js project and all of its artifacts, including source code, configurations, icons, databases, distributables, cordova and electron apps (using the Quasar Framework). Any change to any of the files or spec will work its way "upstream" and "downstream", thus making it possible to change the spec by writing code.

> In the immediate future, there will also be a "shadow runner" that attempts to use machine learning instead of handmade translators.

### Background
A single source of truth (SSoT) is a good beginning to any software project, but the hard work usually lies in actually writing the code. "Modern" project development paradigms focus on task isolation, the reinforcement of managerial structures and a type of workflow that tries to turn people into machines. This project is different.

### Stages of Truth
```
0. Define the semantics of the SPECifications
1. Write a well-formed SPEC
2. Choose your DESTinations
 a. Unit Tests
 b. e2e Tests
 c. Front End
 d. Back End
 e. Messaging
 f. Storage
 g. Content
 h. Documentation
 i. CI Pipeline
 j. ML Training Set
3. Write TRANslators 
4. Generate MANIfest
5. Run truth
```

## 0. Define the semantics of the SPECifications
The semantics of your structure will make it possible for you to read and write the specification of the entire system (or systems if there are several). With prototyping you can define classes and inheritance and use domains to link similar concepts - even other domains. The yaml structure is a good approach in that it is relatively free of unimportant characters and is easy for a human to flyover and immediately recognize inheritance. 

If you click below on **Details**, you will see an example custom symbol definition spec, worked out in rather painful detail. The point of the exercise is to define a set of symbols that can be used as abstractions, leading to a set of tests for constructing and parsing binary & boolean values that map exquisitely to natural language. This work accidentally revealed a novel method of tracking signed zeros and superposited eigenstates within the undefined boolean value. Fun stuff!
 
<details>

#### Terms
```
# Symbols for Truth File Usage
# Spec 0.0.1 
# Assumes yaml-esque conventions
#
#
#
### meta__________________// not yet turing complete
    %    definition
    /    comment          
    *    placeholder

### scaffolding___________// descriptive 
    ∞    set of all sets  // the entire body of truth
    %    set              // one set is by nature a definition
    °    portion of set   // one set is a portion of all sets.
    :    group            // linkage to similar types, groups, sets
    .    chain            // connector between horizontal siblings
    ,    member           // context aware member of a type, group, set
    
### members_______________// referential  
    <    previous         // used as if, input
    >    next             // used as then, output
    ^    parent 
    ;    child
    ,    sibling          // sibling is an equivalent member
    .    self             // smallest link in a chain
    
### groups________________//
    <>   group            // can also be implicit
    []   class
    ()   function
    {}   variable

### typecasting___________// 
    $   string            // non-numeric data
    #   number            // always assumes the largest possible resolution
    _   boolean           // cast type as an explicit boolean
    
### boolean qualifier
    _0   boolean false    // boolean type with falsealso a meta value
    _1   boolean true     // also a meta value
    _*   boolean          // meta truth value 
    _?   undefined        
    !    false
    !!   true           
    ?!   truthy           // fuzzy / undefined truth / uncollapsed truth matrix
    !?   falsey           // fuzzy / undefined false / uncollapsed truth matrix
         
### comparators___________// actually one nor is enough...    
    <    if              
    <!   not if           // <! A : B > is .nor(A,B) = .not(A).or(B)
    <!!  only if
    >    then
    !>   then not
    !!>  only else
    <!>  else if
    :    or
    !    not              // same as false
    !:   not or           // (neither)
    &    and 
    &!   and not              
    &:   and or     
    \    while
        
### operators_____________// actively brnfckng you since 2018
    +    push             // +1 add true = define = create
    -    pull (pop)       // -0 remove false = undefined = destroy
    |    bridge           // chain unrelated groups I< | > O
    ≈    equivalence      // for passing member traits
    =    exact            // as prefix means calculate
 
    +<   read from
    >+   write to
    _?   

### lambda symbols________//
    V    variables
    Λ    lambda expressions
    λ    lambda
    .    dot
    ()   parenthesis
    :=   substitute       // written E[V := R]
    |    or
          
### helpers_______________// 
    "*"  expansion        // expand except explicit literals
    '*'  literal          // no coersion        
    
EXAMPLES:
  
 Boolean Construction
  _? < _0 !: _1      Undefined if neither false nor true
  _? <! _0 : _1      Undefined if both not false and not true    
  _1 : _? !> _0      Either true or undefined then not false
  _1 <! _0 : _?      True if both not false and not undefined
  
 True / False Meta Boolean
  _* < _?:_0:_1 >     Meta boolean can be undefined, false or true)
  !_1* > _0*
  !!_0* > _1*     
  
 Definition of undefined
  // If "*" is not true and not false then "*" is undefined.
  _1 & _0 <! $* > _* = ?          
  _? = _* < $* !> 1 & 0
  
  This means that truth is either explicit or undefined.
  
  Binary definitions of truth, false, truthy and falsey
  !_1* > _0*       true push false  =  false
  _0+!=0       false push false = false
  0+1=1       false push true  = true
  1+1=1       true push true   = true
  
  a false zero is a one (falsey false is true)
  
  Undefined
  1-1=?       true pull true   = undefined
  0-0=?       false pull false = undefined
  0-1=?       False pull true  = undefined
  1-0=?       True pull false  = undefined
  0+0=0       zero push zero = still zero
  0-0=?       (zero pop zero = undefined)       
  -1+1=-0     (negative signed zero is a post-truthy false)
  +1-1=+0     (positive signed zero is a post-truthy false)
       
  // While the string "*" exists, then its meta-boolean is true
  \ $* !! > 1*     
  \ $* !! > _* = _1
  ? * ! > * !!             // If "*" is false, then truthify "*" (1*).
  ? 1 ! :0   
```

</details>

Or you could just build everything using TAP.


## 1. Write a well-formed SPEC

The form of domain:subject:predicate:object is modular enough to really describe any kind of specification.

```
### Schema classes
    [@]  domain
    [~]  subject
    [∆]  predicate
    [Ω]  object
    [L]  language
    [R]  runtime
    
@identity ~input.type(text).val(null) ∆has Ωref('Full-Name')
```

This looks like weird stuff, but that is because it is a prototype for:
- a vue constructor in Quasar for an input form field
- a unit test in Mocha-Chai-Sinon
- an e2e test in Cypress flavor
- documentation via JSDoc
- translation keys
- a database
- ...

## 2. Choose your DESTinations

Destinations are like build pipelines in docker configs. By determining your targets / runtimes, it is possible for you to actually write translating code generation scripts - but you can't write any translators until you have a spec. For the sake of argument, we are going to assume that our target languages are html, css, commonjs, vue and es6 and our target runtimes are nodejs, babel, webpack, quasar, cypress and jest.

## 3. Write TRANslators
By using this "single source of truth" as an input and four different "translating" engines as output, you get everything at one time...

<details>

A cypress test:

```js
  it('has a basic form that works as intended', () => {
    // this is a translator pattern 
    // the same approach should be used in the form.vue
    // for construction

    Object.keys(formdata).forEach((key) => {    
      if (formdata[key]) {
        const dataCy = lowercase(formdata[key].generic);
        const placeholder = formdata[key].placeholder || hyphenToSpace(formdata[key].generic);
        const path = hyphenToNull(toCamelCase(formdata[key].generic));
        const value = parseInt(formdata[key].valid, 10);    
        cy.get(`[data-cy=${dataCy}] .q-input-target`)
          .should('have.attr', 'placeholder', placeholder)
          .type(value)
          .should('have.value', value)
          .log('local storage should be set')
          .should(() => {
            expect(testStorage(path)).to.eq(value); 
          });
        getStore().its(storePather(path)).should('eq', value);  
      } 
    }); 
  });
```

a store description

```json
      "fieldLabels": {
        "contactInfo": [
          {
            "id": "fullName",
            "label": "Full Name:",
            "val": null,
            "type": "text",
            "placeholder": "Full Name",
            "dataCy": "full-name",
          },
```

some translation helpers

```js
const toCamelCase = (str) => str.toLowerCase().replace(/(?:^\w|[A-Z]|\b\w)/g, (ltr, idx) => idx === 0 ? ltr.toLowerCase() : ltr.toUpperCase()).replace(/\s+/g, '');
const hyphenToSpace = (str) => str.replace(/-/g, " ");
const hyphenToNull = (str) => str.replace(/-/g, "");
const lowercase = (str) => str.toLowerCase()
```

a state constructor for vuex

```js
const helpers = require('../helpers');
const truth = require('../json/truth_generated.json');
letcontact = {};

let objectPath = truth.data.summary.fieldLabels.contactInfo;
for (let type in objectPath) { // not sure if this can be so simple in the browser
 contact[toCamelCase(objectPath[type].generic)]=objectPath[type].value
}

export default {
  data: {
    applicationComplete: false,
    contactInfo: contact,
    ...
```

form
```html
<truth>
    <q-input for="input in store.get.data.contactInfo.inputs" placeholder="`input.placeholder`" v-model="`input.type`">
</truth>
```

</details>


## 4. Generate Manifest

The manifest is actually a multidimensional Data-Flow Graph in which the representation of each node in each dimension is an operation (op) of lambda calculus undertaken as defined by the system's current state of truth. An op has zero or more inputs, zero or more outputs and internal state that may be defined by constants or variables of any type (object, function, boolean). Edges between ops are data flows from and to these operations. TBC
 


```yaml
Chaining Truth
    # Uptruth      Push truth upward toward the spec (aka Truth proxy)
     [$t.up(*)] 
    # Downtruth    Push truth downward from the spec (implicit movement)
     [$t.down(*)]
    # Floodtruth   Push truth upward and downward (testing manoeuvre) 
     [$t.flood(*)]
```

## 5. Run Truth

Actually the fun part. This is when you get to go get coffee, take a break. While you are out, you might realize that the spec needs to be changed. So you get back, rewrite the spec and then regenerate the manifest and send the job to the truth runner. Of course, on a rainy day, you might even decide to rework some of the translators. Tree-shaking and lambda substitution take care of all of the hard work for you - and because the manifest has not changed, you just need to set the runner loose...


The truth runner 



## Resources

#### Translating
- awk as a CLI tool for transformations
- http://breuleux.net/blog/language-howto.html
- [earl-grey](https://github.com/breuleux/earl-grey)

#### OpenAPI / schema foundation
- https://swagger.io/specification/

#### Vue / Quasar
- Think about a custom "vue component template type": `<truth>`.
- https://github.com/QingWei-Li/vuep (interactive building)
- https://github.com/bloodf/Quasar-StoreRoutes (create routes from the store)
- https://github.com/tomers/vue-form-generator (is currently being patched to work with quasar)
- https://github.com/quasarframework/quasar-icon-factory
- https://github.com/quasarframework/quasar-testing/projects/2

#### interface
- http://wiredjs.com/ cute
- pug (was jade) - uses mixins, compliant with quasar

#### i18n
- A nice VUEX version of i18n: https://github.com/dkfbasel/vuex-i18n

#### Docs Generation
- https://github.com/QingWei-Li/docsify/ (on the fly from markdown)
- https://vuepress.vuejs.org (pure vue)
- https://storybook.js.org/addons/addon-gallery/
- https://sourcey.com/spectacle/ (OpenAPI/Swagger compliant)
- https://documentjs.com

#### Tests
- https://github.com/storybooks/storybook/tree/master/addons/jest (live storybook jest FTW!)

#### Functional Programming
- https://github.com/getify/Functional-Light-JS
- https://github.com/fantasyland/fantasy-land
- https://github.com/trekhleb/javascript-algorithms

#### WebAssembly
- asm.js - see colin eberhardt
- emscripten => openGL => webGL (game engine stuff)
- [webassembly syntax](https://webassembly.github.io/spec/core/syntax/index.html)
- https://github.com/ballercat/walt js style webassembly 
- https://github.com/AssemblyScript/assemblyscript
- Rust => webassembly
- asmdom

#### ML 
- Intel nGraph
- https://github.com/mila-udem/myia

#### Science
- https://nautil.us/blog/how-brain-waves-surf-sound-waves-to-process-speech

### Contributors
@nothingismagick
