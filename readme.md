# Autocomplete, Multi Select and Search select

> This components are created to use in Vue applications. 

# Table of Contents

* [autocomplete](#autocomplete)
* [multi select](#multi-select)
* [search select](#search-select)

## autocomplete

> The autocomplete implements a input where you can complete the words to search in a list of elements. Returning a object with the id and value of the result.

### Getting started

* Add the component into your app:

```javascript
  import autocomplete from 'path/autocomplete.vue'
  ```

* Props
    - `value`: Object or text for search
    - `source`: 
    - `placeholder`:

* Use the `autocomplete` component:

```html
    <script>
        import autocomplete from "@/components/autocomplete.vue";

        export default{
            components: {
                autocomplete,
            },
            data(){
                return {
                    value: {}
                }
            }
        }
    </script>

    <template>
        <autocomplete v-model="value" source="" placeholder="Enter your text">     
        </autocomplete>
    </template>
```

### dependencies
- vue
- bootstrap
- axios

## multi select

> The multi select implements a input where you can search and display the list by categories.

### dependencies
- vue

## search select

> The search select implements a input where you can search based on your list of elements.

### dependencies
- vue
