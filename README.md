# truth-runner

A single source of truth (SSoT) is a good beginning to any software project, but the hard work usually lies in actually writing the code. "Modern" project development paradigms focus on task isolation, the reinforcement of managerial structures and a type of workflow that tries to turn people into machines. This project is different.

## Stages of Truth
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
3. Write TRANslators 
4. Generate MANIfest
5. Run truth
```

### 0. Define the semantics of the SPECifications
The semantics of your structure will make it possible for you to read and write the specification of the entire system (or systems if there are several). With prototyping you can define classes and inheritance and use domains to link similar concepts - even other domains. The yml structure is a good approach in that it is free of unimportant characters and is easy for a human to flyover. 

The form of domain:subject:predicate:object is modular enough to really describe any kind of specification.

```

@identity ~input.type(text).val(null) ∆has Ωref('Full-Name')
```

This looks like weird stuff, but that is because it is a prototype for:
- a vue constructor in Quasar
- a unit test in Mocha-Chai-Sinon
- an e2e test in Cypress flavor
- documentation vis JSDoc(edited)
- translation keys
- a database

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

### Terms
```yaml
# Truth File Definition
# Assumes yaml conventions


# Structural operators

  //   comment
  %    definition
  =    equivalence 
  *    placeholder
  °    set of all sets
  %    portion of set
  
  ,    group
  .    chain
  <    previous chain 
  >    next chain    
  -    child
  ^    parent 
  +    new
  "    expansion 
  '    literal
  []   class
  ()   function
  {}   variable
  !    type 
  !$   string
  !#   number
  !0   boolean
  0    false
  1    true

# Schema classes
 
  [@]  domain
  [~]  subject
  [∆]  predicate
  [Ω]  object
 

Chaining Truth
 # Uptruth:     Push truth upward toward the spec (aka Truth proxy)
  [$t.up(*)] 
 # Downtruth:   Push truth downward from the spec (implicit movement)
  [$t.down(*)]
 # Floodtruth:  Push truth upward and downward (testing manoeuvre) 
  [$t.flood(*)]
```

## Resources

#### OpenAPI / schema foundation
- https://swagger.io/specification/

#### Vue / Quasar
- Think about a custom "vue component".
- https://github.com/bloodf/Quasar-StoreRoutes
- https://github.com/QingWei-Li/vuep (interactive building)

#### interface
- http://wiredjs.com/ cute

#### i18n
- A nice VUEX version of i18n: https://github.com/dkfbasel/vuex-i18n

#### Docs Generation
- https://github.com/QingWei-Li/docsify/ (on the fly from markdown)
- https://vuepress.vuejs.org (pure vue)
- https://storybook.js.org/addons/addon-gallery/
- https://sourcey.com/spectacle/ (OpenAPI/Swagger compliant )

#### Functional Programming
- https://github.com/getify/Functional-Light-JS
- https://github.com/fantasyland/fantasy-land
- https://github.com/trekhleb/javascript-algorithms

### Contributors
@nothingismagick
