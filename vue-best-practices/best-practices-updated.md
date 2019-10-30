Vue Best Practices:
-
**Naming Conventions​:** (mostly from style guide documentation and simple articles)  

 - Use kebab-case for event names (things that will be emitted/listened for); vue converts them into lowercase anyways [(link)](https://blog.usejournal.com/vue-js-best-practices-c5da8d7af48d)
 
 - Prop names should use camelCase during declaration, but kebab-case in templates; camelCase is standard for JavaScript (used for declarations), while kebab-case is standard for html (used in templates) [(link)](https://blog.usejournal.com/vue-js-best-practices-c5da8d7af48d)

 - Component names should always be multi-word (except root “App” components) [(link)](https://itnext.io/how-to-structure-a-vue-js-project-29e4ddc1aeeb)
	 - ex. UserCard instead of Card

 - Components that are only used once should begin with the prefix “The” [(link​)](https://itnext.io/how-to-structure-a-vue-js-project-29e4ddc1aeeb) 
	 - ex. “TheFooter.vue” if footer can only be used once

 - Child components should include parent name as prefix [(​link​)](https://itnext.io/how-to-structure-a-vue-js-project-29e4ddc1aeeb) 
	 - ex. “UserCardPhoto” where “Photo” component is used in “UserCard”

**Basic Tip​s:** (also mostly from vue style documentation)

 - Always add :key in v-for loops [(​link)​](https://blog.usejournal.com/vue-js-best-practices-c5da8d7af48d)

 - Never use v-if on the same element as v-for [(​link​)](https://blog.usejournal.com/vue-js-best-practices-c5da8d7af48d)

 - For single file components, use scoped ccs style [(​link​)](https://vuejs.org/v2/style-guide/?source=post_page-----29e4ddc1aeeb----------------------)
        
**Computed vs Methods​:** [(​link​)](https://www.sitepoint.com/the-difference-between-computed-properties-methods-and-watchers-in-vue/)
- Methods: function that’s a property of an object, used to group common functionality, like handling form submissions or ajax requests

``// in component ``
``methods: {``
``r​everseMessage​: f​unction​() {​ r​eturn​t​his.​message.split(​'')​.reverse().join(​'')​``
``} }``

- Computed: doing something new to a set of data, like filtering or combining multiple things, stored in cache

``computed​: {  ``
``/​/ a computed getter reversedMessage: ​function​() {​``
``/​/ `this` points to the vm instance``
``r​eturn​t​his.​message.split(​'')​.reverse().join(​'')​ }``

- Don’t use methods when you could use computed – computed properties are stored in cache and only re-evaluated when dependencies change, while methods will run whenever a re-render happens [(​link​)](https://vuejs.org/v2/guide/computed.html%23Computed-Caching-vs-Methods)
	- For the above example of reversing text, computed is better because it will only run when the dependencies change (if message stays the same, it won’t have to re-run)

**Watchers​:** [(​link​)](https://vuejs.org/v2/guide/computed.html%23Watchers)
- Watchers: used to perform asynchronous operations only when necessary; good for operations that respond to changing data and that could be expensive if using computed

``<div id=​"watch-example">​ <​p>``
 `Ask a yes/no question: `
`<​input v-model=​"question">​ <​/p>`
`<​p>{​{ answer }}​</p> </div>`

- Using {immediate: true}: triggers a watcher to be called immediately upon running [(​link​)](https://vuejs.org/v2/api/%23vm-watch-expOrFn-callback-options)[(example)](https://stackoverflow.com/questions/45354027/vue-js-how-to-fire-a-watcher-function-when-a-component-initializes)

``(example) 'a'``
``i​mmediate​: t​rue })``
``// `callback` is fired immediately with current value of 'a'``

**Keep Alive​:** [(​link​)](https://vuejs.org/v2/guide/components-dynamic-async.html)
1. Wrap component tag with keep alive to cache an instance of a component, so it only needs to be mounted once

``<!-- Inactive components will be cached! -->``
``<keep-alive>``
``<​component v-bind:is=​"currentTabComponent"​></component>``
``</keep-alive>``

**Smart Watchers:** [(link)](https://www.vuemastery.com/conferences/vueconf-us-2018/7-secret-patterns-vue-consultants-dont-want-you-to-know-chris-fritz/)
 - Smart Watchers: use one method that gets called on component creation, and then again when a watched property changes ; 

`

    created() {
	    this.fetchUserList()
    },
    watch: {
	    searchText() {
		    handler: ‘fetchUserList’,
		    immediate: true
	    }
	}

