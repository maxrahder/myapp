## Froala WYSIWYG editor in ExtSJ 7.0 Early Access

The ExtJS 7.0 had a Premium code package for the Froala WYSIWYG editor. 

### Installation

You will need a current version of **Sencha Cmd** for any serious work on the SDK. To get
the current development build, download and install the lastPinned build from
[Team City](https://teamcity.sencha.com/viewLog.html?buildTypeId=Cmd_60x_Continuous&buildId=lastPinned).

### Licensing

Setup Git and learn our [Git Workflow](docs/internal/workflow.md).

###Using the Froala Editor

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
property. 

Note that `value` is HTMl and therefore, will contain HTML tags. 

        myFroalaComponent.setValue('Hello world!');
        console.log(myFroalaComponent.getValue()); // Logs "<p>HelloWorld!</p>"

    @example
    Example.main.Main({
        extends: 'Ext.Panel',
        viewModel: {
            data: {
                html: '<p>Four score and seven years ago.</p>'
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
                value: '{foo}' // value is also the default bind property
            }
        }]
    });
 
    Ext.application({
        name: 'Example',
        mainView: 'Example.main.Main'
    });

If used in a form, you can use the field version. Its name-value pair will be relfected in form submits, when
calling `getValue()` on the form.

    @example
    Example.main.Main({
        extends: 'Ext.form.Panel',
        layout: 'fit',
        items: [{
            xtype: 'froalaeditor',
            value: 'Hello world!'
        }],
        buttons: [{
            
        }]
    });
 
    Ext.application({
        name: 'Example',
        mainView: 'Example.main.Main'
    });
    
    
####Froala instance configuration


####Events



####Running Froala native methods



####Specifying a Froala activation key
