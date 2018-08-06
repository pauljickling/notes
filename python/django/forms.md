## Creating Forms in Django

Django has a `ModelForm` module that can easily create forms that are bound to specific models.

The ModelForm module is used in your views.py file inside the necessary request function by creating a class that can be used.

```
class {form name}(ModelForm):
    class Meta:
        model = {Model name}
        fields = [{fields used}]
```

The ModelForm module will utilize what are typically appropriate forms. If you need to change some properties of the forms, you can use the widgets object.

```
widgets = {
    '{field name}': Textarea(attrs={'cols': 80, 'rows': 20}),
}
```

In the above example we would change a field that creates a text area form from the default columns and rows to the 80 columns and 20 rows specified here.

Assuming you are using a template to render your HTML, you would then consume the ModelForm with your context object.

```
context = {
    '{form name}': AnswerForm,
}
```

You will also need to write some code that validates your form data:

```
if request.method == "POST":
    {form name} = {ModelForm name}(request.POST)

    if {form name}.is_valid():
        instance = {form name}.save(commit=False)
        instance.save()
        return render(request, '{url redirect}', context)
```

If there was data for your model not included in your form, you could define it here too. One simple example is your model might contain a created_at field where you want the time/date stamp from when it was submitted.

`instance.created_at = timezone.now()`
