django-ajax-upload-widget
=========================

Provides AJAX file upload functionality for FileFields and ImageFields with a simple widget replacement in the form.


Features
--------

* Drop-in replacement for Django's built-in `ClearableFileInput` widget (no change required to your model).
* Works in Internet Explorer 7 and higher (not tested in IE 6, but may work).
* Random hash string added to file names to ensure uploaded file paths are not guessable by others.


Usage
-----

### Server Side

In your form, use the `AjaxClearableFileInput` on your `FileField` or `ImageField`.

    from django import forms
    from ajax_upload.widgets import AjaxClearableFileInput

    class MyForm(forms.Form):
        my_image_field = forms.ImageField(widget=AjaxClearableFileInput())


Or, if using a `ModelForm` you can just override the widget.

    from django import forms
    from ajax_upload.widgets import AjaxClearableFileInput

    class MyForm(forms.ModelForm):
        class Meta:
            model = MyModel
            widgets = {
                'my_image_field': AjaxClearableFileInput
            }


### Client Side

Include the Javascript (and optionally CSS) files in your page and call the `autoDiscover` function.
This will search the page for all the AJAX file input fields and apply the necessary Javascript.

    <link href="{{ STATIC_URL }}ajax_upload/css/ajax-upload-widget.css" rel="stylesheet" type="text/css"/>
    <script src="{{ STATIC_URL }}ajax_upload/js/jquery.iframe-transport.js"></script>
    <script src="{{ STATIC_URL }}ajax_upload/js/ajax-upload-widget.js"></script>

    <script>
        $(function() {
            AjaxUploadWidget.autoDiscover();
        });
    </script>


You can also pass options to `autoDiscover()`:


    <script>
        $(function() {
            AjaxUploadWidget.autoDiscover({
                changeButtonText: 'Click to change',
                onError: function(data) { alert('Error!'); }
                // see source for full list of options
            });
        });
    </script>


OR ... you can explicitly instantiate an AjaxUploadWidget on an AJAX file input field:

    <input id="Foo" name="foo" type="file" data-upload-url="/ajax-upload/" data-filename="" data-required=""/>
    <!-- The input field needs to be outputed by Django to contain the appropriate data attributes -->

    <script>
        new AjaxUploadWidget($('#Foo'), {
            // options
        });
    </script>



Dependencies
------------
* jQuery
* jQuery Iframe Transport plugin (included in this package)


Installation
------------

1. Add `ajax_upload` to your `INSTALLED_APPS` setting.

1. Hook in the urls.

        # urls.py
        urlpatterns += patterns('',
            (r'^ajax-upload/', include('ajax_upload.urls')),
        )

1. Run the tests to verify it is working.

        ./manage.py test ajax_upload

1. That's it (don't forget include the Javascript as mentioned above).
