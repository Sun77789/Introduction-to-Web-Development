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