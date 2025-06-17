# Homework 5: Databases

Follow the [instructions](https://github.com/Tech-at-DU/ACS1710-Web-Architecture/blob/master/Assignments/04-Databases.md) to complete this assignment!

This project uses restful route mapping: 

| Purpose                | Method | Route                         | View Function       |
| ---------------------- | ------ | ----------------------------- | ------------------- |
| List all plants        | GET    | `/plants`                     | `list_plants()`     |
| Show form to add plant | GET    | `/plants/new`                 | `new_plant_form()`  |
| Create a new plant     | POST   | `/plants`                     | `create_plant()`    |
| View a specific plant  | GET    | `/plants/<plant_id>`          | `show_plant()`      |
| Edit a plant (form)    | GET    | `/plants/<plant_id>/edit`     | `edit_plant_form()` |
| Update a plant         | POST   | `/plants/<plant_id>` (or PUT) | `update_plant()`    |
| Delete a plant         | POST   | `/plants/<plant_id>/delete`   | `delete_plant()`    |
| Create a harvest       | POST   | `/plants/<plant_id>/harvests` | `create_harvest()`  |



