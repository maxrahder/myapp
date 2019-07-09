## Froala WYSIWYG editor in ExtSJ 7.0 Early Access

The ExtJS 7.0 has a Premium code package that lets you us the Froala WYSIWYG editor within your applications.


There are two versions of the editor: a field version, for use in forms, and a regular component version, used
when you don't need its value to be included in a form submit.

Both classes are simple wrappers around the Froala WYSIWYG editor, documented at https://www.froala.com/wysiwyg-editor.
Since these are ExtJS components, you can use them like any other ExtJS component, including setting up listeners
to detect both component events and Froala native events. You can also run native Froala methods directly on the 
Froana instance.

### Installation


### Licensing


### Using the Froala Editor

There are two versions of the Froala Editor: 
- A component version &mdash; `Ext.froala.Editor`
- A field version &mdash; `Ext.froala.EditorField`

These components are wrappers around a Froala Editor instance. They are configured and used identiaclly, 
but the field version extends `Ext.field.Field`, and consequently, can be given a `name` and `value`, 
and be used in field panels and form panels.

#### Basic usage

There are two primary configs: `value`, which is the HTML value of the editor, and `editor`, which is the 
configuration for the Froala Editor instance being created.

    @example
    Example.main.Main({
        extends: 'Ext.Panel',
        layout: 'fit',
        items: [{
            xtype: 'froalaeditor',
            value: 'Hello world!'
        }]
    });
 
    Ext.application({
        name: 'Example',
        mainView: 'Example.main.Main'
    });

### The value config

The `value` config specifies the initial value of the editor. `value` is bindable and is the default bind
property. Note that `value` is HTMl and therefore, will contain HTML tags. 

    myFroalaComponent.setValue('Hello world!');
    console.log(myFroalaComponent.getValue()); // Logs "<p>Hello world!</p>"

    @example
    Example.main.Main({
        extends: 'Ext.Panel',
        viewModel: {
            data: {
                html: '<p>Hello world!</p>'
            },
            formulas: {
                encodedHtml: function(get) {
                    return Ext.htmlEncode(get('html'));
                }
            }
        },
        title: 'Froala Editor',
        tbar: [
            {
                xtype: 'label',
                bind: {
                    html: '{html}'
                }
            }
        ],
        bbar: [
            {
                xtype: 'label',
                bind: {
                    html: '{encodedHtml}'
                }
            }
        ],
        layout: 'fit',
        items: [{
            xtype: 'froalaeditor',
            bind: {
                value: '{html}' // value is also the default bind property
            }
        }]
    });
 
    Ext.application({
        name: 'Example',
        mainView: 'Example.main.Main'
    });

Within a form you can use the field version. Its name-value pair will be relfected in form submits, or when
calling `getValue()` on the form.

    @example
    Example.main.Main({
        extends: 'Ext.form.Panel',
        layout: 'fit',
        items: [{
            xtype: 'froalaeditor',
            name: 'html',
            value: 'Hello world!'
        }],
        buttons: {
            ok: {
                text: 'Submit',
                handler: function(button) {
                    // From here, running "submit()" will include "html", and its value, as a query field.
                }
            }
        }
    });
 
    Ext.application({
        name: 'Example',
        mainView: 'Example.main.Main'
    });
    
    
#### Froala instance configuration

The `editor` config lets you configuration options for the Froala editor instance. You can use any Froala config, 
as documented at https://www.froala.com/wysiwyg-editor/docs/options

    @example
    Example.main.Main({
        extends: 'Ext.Panel',
        layout: 'fit',
        items: [{
            xtype: 'froalaeditor',
            value: 'Hello world!',
            editor: {
                autofocus: true,
                fontSize: ['10', '12', '16', '24']
            }
        }]
    });
 
    Ext.application({
        name: 'Example',
        mainView: 'Example.main.Main'
    });

#### Events

To listen to events, use the standard `listeners` component config. You can listen to native Froala events
by using the _froala_ prefix on the event name. Froala events are docuemnted at 
https://www.froala.com/wysiwyg-editor/docs/events.

This example shows a Froala editor configured with listener for its _change_ event, and in addition, a
listener to the native Froala _click_ event, specified by using the _froala-+ prefix.

    @example
    Example.main.Main({
        extends: 'Ext.Panel',
        layout: 'fit',
        items: [{
            xtype: 'froalaeditor',
            value: 'Hello world!',
            listeners: {
                change: function(froalaComponent) {
                    Ext.toast({ message: 'Change!' });
                },
                // Native Froala events are prefixed with 'froala.'
                'froala.click': function(froalaComponent) {
                    Ext.toast({ message: 'Click!' });
                }
            }
        }]
    });
 
    Ext.application({
        name: 'Example',
        mainView: 'Example.main.Main'
    });


#### Running Froala native methods

To run native Froala methods, use `getEditor()` to get a reference to the Froala instance, then
run any method you wish. Froala methods are documented at https://www.froala.com/wysiwyg-editor/docs/methods.

For example, to create a 

### Startup time

When the `FroalaEditor` or `FroalaEditorField` is created, it takes a few milliseconds for the wrapped Froala instance
to be created and initialized. When setting up events, this is transparent, but if you need to detect when the instance
is ready, use the _ready_ event. The instance also has a _isReady_ boolean property that starts out _false_, and changes
to _true_ when the component is initialized.

This code illustrates the relationship between the property and event.

    @example
    Example.main.MainController({
        extends: 'Ext.app.ViewController',
        init: function(){
            console.log(this.lookup('froalaeditor').getEditor().isReady); // Logs false
        },
        onReady: function(){
            console.log(this.lookup('froalaeditor').getEditor().isReady); // Logs true
        }
    });
    
    Example.main.Main({
        extends: 'Ext.Panel',
        controller: {
            xclass: 'Example.main.MainController'
        },
        layout: 'fit',
        items: [{
            xtype: 'froalaeditor',
            reference: 'froalaeditor',
            listeners: {
                ready: 'onReady'
            }
        }]
    });
 
    Ext.application({
        name: 'Example',
        mainView: 'Example.main.Main'
    });
    
#### Specifying a Froala activation key

To use a licensed copy of the Froala Editor, you need an _activation key_, as documented at
https://wysiwyg-editor.froala.help/hc/en-us/articles/115000394945-What-is-an-Activation-Key-

You then specify the key in your applications `app.json`, within a config block named `froala`. This 
is an example that shows a section of `app.json` with the `requires` entry for the `froana-editor`
code package, as well as the specification for the activation key.

    {
        "name": "MyApp",
        "namespace": "MyApp",
        "framework": "ext",
        "requires": ["font-awesome", "froala-editor"],
        "froala" {
            "activation-key": "my-activation-key"
        }
        ...
    }


