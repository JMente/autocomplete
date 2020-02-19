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
    - `value`: Object or text for search in the autocomplete
    - `source`: The endpoint which the autocomplete connects
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
        <autocomplete 
            v-model="value" 
            source="" 
            placeholder="Enter your text">     
        </autocomplete>
    </template>
```

### dependencies
- vue
- bootstrap
- axios

## multi select

> The multi select implements a input where you can search and display the list by categories.

### Getting started

* Add the component into your app:

```javascript
  import multiSelect from 'path/multi-select.vue'
  ```

* Props
    - `value`: 
    - `options`: 
    - `placeholder`:
    - `depend`:

* Use the `multi-select` component:

```html
    <script>
        import multiSelect from "@/components/multi-select.vue";

        export default{
            components: {
                multiSelect,
            },
            data(){
                return {
                    value: {}
                }
            }
        }
    </script>
    <template>
        <multiSelect 
            v-model="value" 
            :options="" 
            placeholder="Search your option"
            depend="">     
        </multiSelect>
    </template>
```

### dependencies
- vue

## search select

> The search select implements a input where you can search based on your list of elements.

### Getting started

* Add the component into your app:

```javascript
  import searchSelect from 'path/search-select.vue'
  ```

* Props
    - `value`: 
    - `options`: 
    - `placeholder`:
    - `searchable`:

* Use the `search-select` component:

```html
    <script>
        import searchSelect from "@/components/search-select.vue";

        export default{
            components: {
                searchSelect,
            },
            data(){
                return {
                    value: {}
                }
            }
        }
    </script>
    <template>
        <searchSelect 
            v-model="value" 
            :options="" 
            placeholder="Search your option"
            :searchable="true"
            >     
        </searchSelect>
    </template>
```

### dependencies
- vue
