<ul>

<li>
1. Create new model
> rails g resource Product name price:decimal <br />
> rake db:migrate <br />
</li>

<li>
2. Add validations to the Model in app/models/product.rb. <br />
<code>
class Product < ActiveRecord::Base
  validates :name, presence: true
  validates :price, presence: true,
            numericality: true,
            format: { :with => /\A\d{1,4}(\.\d{0,2})?\z/ }
end
</code>
</li>

<li>
3. Populate the database and then: 
> rake db:seed
<code>
  Product.delete_all <br />

  Product.create!([ <br />
    {id: 1, name: "Nintendo Wii U Premium", price: 250}, <br />
    {id: 2, name: "XBox 360 250GB", price: 250}, <br />
    {id: 3, name: "Playstation 3 500 GB", price: 239.95}, <br />
    {id: 4, name: "Nintendo Wii", price: 99.95}, <br />
    {id: 5, name: "Nintendo 3DS", price: 174.95} <br />
  ])
</code>
</li>

<li>
4. Add CRUD to the Products controller in app/controllers/products_controller.rb
<code>
  class ProductsController < ApplicationController
  
  def index
    @products = Product.all
  end

  def show
    @product = Product.find(params[:id])
  end

  def new
    @product = Product.new
  end

  def create
    @products = Product.all
    @product = Product.create(product_params)
  end

  def edit
    @product = Product.find(params[:id])
  end

  def update
    @products = Product.all
    @product = Product.find(params[:id])
    
    @product.update_attributes(product_params)
  end

  def delete
    @product = Product.find(params[:product_id])
  end

  def destroy
    @products = Product.all
    @product = Product.find(params[:id])
    @product.destroy
  end

private
  def product_params
    params.require(:product).permit(:name, :price)
  end
end
</code>
</li>

<li>
4. Set up the routes in config/routes.rb:
<code>
  THENAMROFYOURAPP::Application.routes.draw do
  resources :products do
    get "delete"
  end

  root to: "products#index"
end
</code>
</li>

<li>
  <h4>Adding Views</h4>
</li>

<li>
5. Create an index view and add to app/views/products/index.html.erb::
<code>
  <div class="container">
  <div class="well">
    <%= link_to "New Product", new_product_path, remote: true, class: "btn btn-primary" %>
  </div>
  <div class="new-product"></div>
  
  <table class="table table-bordered table-striped">
    <thead>
      <tr>
        <th>Name</th>
        <th>Price</th>
        <td>&nbsp;</td>
        <td>&nbsp;</td>
      </tr>
    </thead>
    <tbody class="product-index">
      <%= render "index" %>
    </tbody>
  </table>
</div>
<div id="product-modal" class="modal fade"></div>
</code>
</li>

<li>
6. Create _index.html.erb partial with code:
<code>
  <% @products.each do |product| %>
    <tr>
      <td><%= product.name %></td>
      <td><%= number_to_currency product.price %></td>
      <td><%= link_to "Edit", edit_product_path(product), remote: true, class: "btn btn-default" %></td>
      <td><%= link_to "Delete", product_delete_path(product), remote: true, class: "btn btn-danger" %></td>
    </tr>
  <% end %>
</code>
</li>

<li>
6. Create a _form.html.erb partial.
<code>
  <div class="modal-dialog">
  <div class="modal-content">
    <%= form_for @product, remote: true, html: { style: "display:inline;" } do |f| %>
      <div class="modal-body">
        <ul class="errors"></ul>

        <div class="form-group">
          <%= f.label :name, class:"control-label" %>
          <%= f.text_field :name, class: "form-control" %>
        </div>
        <div class="form-group">
          <%= f.label :price, class: "control-label" %>
          <%= f.text_field :price, class: "form-control" %>
        </div>
      </div>
      <div class="modal-footer">
        <%= f.submit class: "btn btn-primary" %>
        <%= link_to "Cancel", "#", class: "btn", data: {dismiss: "modal"} %>
      </div>
    <% end %>
  </div>
</div>
</code>
</li>

<li>
7.  Create a _new.html.rb file
<code>
  <div class="modal-header">
  <h3>New Product</h3>
  </div>
  <%= render "form" %>
</code>
</li>

<li>
8. Create an _edit.html.rb file for products 
<code>
  <div class="modal-header">
  <h3><%= "Editing #{@product.name}" %></h3>
  </div>
  <%= render "form" %>
</code>
</li>


<li>
  <h4>Javascript files</h4>
</li>

<li>
9. Create the new.js.erb view inside app/views/products folder.
<code>
  $("#product-modal").html("<%= escape_javascript(render 'new') %>")
  $("#product-modal").modal("show")
</code>
</li>

<li>
10. Create the edit.js.erb view 
<code>
  $("#product-modal").html("<%= escape_javascript(render 'edit') %>")
  $("#product-modal").modal("show")
</code>
</li>

<li>
11. Create a partial that will be shared between the create, update, and destroy js files. This file will determine if the product was successfully saved. 
<code>
$("ul.errors").html("")
<% if @product.errors.any? %>
  <% @product.errors.full_messages.each do |message| %>
    $("ul.errors").append($("<li />").html("<%= message.html_safe %>"))
  <% end %>
<% else %>
  $(".product-index").html("<%= escape_javascript(render 'index') %>")
  $("#product-modal").modal("hide")
<% end %>
</code>
</li>

<li>
12. Create update.js.erb, destroy.js.erb and create.js.erb views, all with the following code: 
<code>
  <%= render "save" %>
</code>
</li>


<li>
  <h4>Delete confirmation</h4>
</li>


<li>
13. Create a partial called _delete.html.erb
<code>
  <div class="modal-dialog">
    <div class="modal-content">
    <div class="modal-header">
      <h3>Delete Product</h3>
    </div>
    <div class="modal-body">
    <ul class="errors"></ul>
    Are you sure you wish to delete <b><%= @product.name %></b>?
    </div>
    <div class="modal-footer">
      <%= link_to "Yes, Delete This Product", product_path(@product), method: :delete, remote: true, class: "btn btn-danger" %>
      <%= link_to "No, Please Don't", "#", class: "btn btn-default", data: { dismiss: "modal" } %>
    </div>
    </div>
  </div>
</code>
</li>

<li> 
14. Create the delete.js.erb javascript view 
<code>
$("#product-modal").html("<%= escape_javascript(render 'delete') %>")
$("#product-modal").modal("show")
</code>
</li>
<ul>
<h5>DONE/h5>