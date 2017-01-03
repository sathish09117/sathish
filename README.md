# sathish
FunDapter takes the pain and hassle out of creating a new Adapter class for each ListView you have in your Android app.

It is a new approach to custom adapter creation for Android apps. You get free ViewHolder pattern support, field validation so you don't get bit by trivial bugs and best of all - you get to keep it DRY!

What's new?

Gradle support now only contains a JAR archive instead of AAR which wasn't needed. Just add compile 'com.github.amigold.fundapter2:library:1.01' to your dependencies in the build.gradle file in your project.
What you used to do:

Subclass BaseAdapter or copy existing adapter you already wrote.
Create a ViewHolder static class a define all the views in it.
Write (Copy.. don't fool yourself!) the whole ViewHolder creation code from somewhere.
Write all the "findViewById" lines.
Start filling data in the views inside the getView method.
Well that was boring! I feel your pain!

What FunDapter lets you do:

Create a new BindDictionary
Add fields.
Create a new FunDapter instance, supplying the BindDictionary, layout resource file and item list.
Getting Started

This is the Product class we'll create an adapter for:

public class Product {

    public String title;
    public String description;
    public String imageUrl;
    public double price;
}
Create a new BindDictionary instance:

BindDictionary<Product> dict = new BindDictionary<Product>();
Adding a basic text field:

dict.addStringField(R.id.description,
    new StringExtractor<Product>() {

        @Override
        public String getStringValue(Product item, int position) {
            return item.description;
        }
    });
Notice how you simply provide the id of the TextView and an implementation of the StringExtractor which will be used to get the correct String value from your Product.

Now a more complicated text field:

dict.addStringField(R.id.title,
    new StringExtractor<Product>() {

        @Override
        public String getStringValue(Product item, int position) {
            return item.title;
        }
    }).typeface(myBoldFace).visibilityIfNull(View.GONE);
Notice how you can chain calls to get some more complex behaviours out of your views. typeface() will set a typeface on the view while visibilityIfNull() will change the visibility of the field according to the value being null or not.

What about our image?? Lets add that as well:

prodDict.addDynamicImageField(R.id.productImage,
    new StringExtractor<Product>() {

        @Override
        public String getStringValue(Product item, int position) {
            return item.imageUrl;
        }
    }, new DynamicImageLoader() {
        @Override
        public void loadDynamicImage(String url, ImageView view) {
            //insert your own async image loader implementation
        }
    });
In here the StringExtractor grabs the URL from the Product item while the ImageLoader gives you a reference to the view and the URL you extracted so you can use your own custom lazy image loading implementation.

Finally, create the adapter:

FunDapter<Product> adapter = new FunDapter<Product>(getActivity(), productArrayList,
        R.layout.product_list_item, dict);
What is supported so far:

ViewHolder pattern and more performance optimizations
Switching data using funDapter.updateData()
Alternating background colors for the list items. Use funDapter.setAlternatingBackground()
Text fields:
typeface
visibility if null
changing textcolor based on a boolean condition - chain conditionalTextColor() when setting the field
Image fields (that load from the web)
ProgressBar fields - for showing user progress or xp.
Conditional visibility views - views that are shown or hidden according to some boolean value. (Good for decorations such as "sale" banners)
All fields support setting an OnClickListener by chaining onClick()
ExpandableListAdapter is supported
What next?

Support for ViewPagerAdapter
Support for Favorite toggle buttons (where you provide your own implementation for the data persistence)
Whatever else I can think of!
