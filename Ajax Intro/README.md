For the guidlines with code, please open the file README.txt instead.
<ul>
<li>
1. Create new model
> rails g resource Product name price:decimal <br />
> rake db:migrate <br />
</li>

<li>
2. Add validations to the Model in app/models/product.rb. <br />
</li>

<li>
3. Populate the database and then: 
> rake db:seed
</li>

<li>
4. Add CRUD to the Products controller in app/controllers/products_controller.rb
</li>

<li>
4. Set up the routes in config/routes.rb:
</li>

<li>
  <h4>Adding Views</h4>
</li>

<li>
5. Create an index view and add to app/views/products/index.html.erb::
</li>

<li>
6. Create _index.html.erb partial with code
</li>

<li>
6. Create a _form.html.erb partial.
</li>

<li>
7.  Create a _new.html.rb file
</li>


<li>
  <h4>Javascript files</h4>
</li>

<li>
9. Create the new.js.erb view inside app/views/products folder.
</li>

<li>
10. Create the edit.js.erb view 
</li>

<li>
11. Create a partial that will be shared between the create, update, and destroy js files. This file will determine if the product was successfully saved. 
</li>

<li>
12. Create update.js.erb, destroy.js.erb and create.js.erb views, all with the following code: 
</li>

<li>
  <h4>Delete confirmation</h4>
</li>


<li>
13. Create a partial called _delete.html.erb
</li>

<li> 
14. Create the delete.js.erb javascript view 
</li>
<ul>
<h5>DONE</h5>