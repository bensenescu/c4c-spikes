    Javascript is a dynamically typed language. This enables errors to only occur at runtime, thus allowing a program to run even if it contains type errors that would otherwise prevent the script from running properly. 

Typescript is a strict syntactic superset of JS that adds optional static typing to the language. It is designed for the development of large JS applications.

Its main advantages include:
* Type safety in refactoring
* Type system for reference
* Type system for prototyping

Vue 

VueJS doesn't include great support for typescript. As of now, the only way to include TS in a vue project is by using Class-Style Vue Components, instead of the traditional Options API. 

This was originally the direction that VueJS was heading for the next release of Vue (3.0). However,Evan You published a Request for Comments (RFC) regarding the new direction for Vue, and in it said the Class API has been discontinued in favor of a new Composition API. 

RFC: 
https://github.com/vuejs/rfcs/pull/42

Read more about the difference between the Class API and the new Composition API: 
https://github.com/vuejs/rfcs/blob/function-apis/active-rfcs/0000-function-api.md#type-issues-with-class-api


For this reason I would advocate not using Typescript, as we will be using the Vue 2.0 Options API.