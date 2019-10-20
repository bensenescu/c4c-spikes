# Naming Conventions
### (mostly from style guide documentation and simple articles)
- use kebab-case for event names (things that will be emitted/listened for)
    - vue converts them into lowercase anyways
- prop names should use camelCase during declaration, but kebab-case in templates
    - camelCase is standard for JavaScript (used for declarations), while kebab-case is standard for html (used in templates)
- component names should always be multi-word (except root “App” components)
    - ex. UserCard instead of Card
- components that are only used once should begin with the prefix “The”
    - ex. “TheFooter.vue” if footer can only be used once
- child components should include parent name as prefix
    - ex. “UserCardPhoto” where “Photo” component is used in “UserCard”

# Basic Tips
### (also mostly from vue style documentation)
- always add :key in v-for loops
- never use v-if on the same element as v-for
- for single file components, use scoped ccs style

# Design Patterns: TODO
 
# Computed vs Methods: TODO
- methods: function that’s a property of an object, used to group common functionality, like handling form submissions or ajax requests
- computed: doing something new to a set of data, like filtering or combining multiple things, stored in cache

# Keep Alive: TODO
