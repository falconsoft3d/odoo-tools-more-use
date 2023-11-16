# Odoo Tools more used

# Actualización
```
/etc/init.d/odoo stop
su - odoo -s /bin/bash
odoo -d db14-spain -u all --stop-after-init --logfile=/dev/stdout
odoo -d db14-spain -u base_bim_2 --stop-after-init --logfile=/dev/stdout
/etc/init.d/odoo start
```

# Model
```
name = fields.Char('Code', default="New", copy=False)
@api.model
    def create(self, vals):
        if vals.get('name', "New") == "New":
            vals['name'] = self.env['ir.sequence'].next_by_code(
                'programmed.payment') or "New"
        programmed_payment = super(ProgrammedPayment, self).create(vals)
        return programmed_payment
```


```
@api.model
    def create(self, vals):
        vals['name'] = self.env['ir.sequence'].next_by_code('bim.recurring.contract') or _('New')
        return super().create(vals)
```
       

# Views

Decoración de la vista tree y view optional
```
<tree string="Programmed Payment"
                            decoration-danger="state == 'draft'"
                            decoration-success="state == 'done'"
                            decoration-muted="state == 'cancel'"
                            delete="false"
                >
<field name="purchase_id" optional="show"/>
```



# Permisos de usuarios
ir.model.access.csv
```
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
access_programmed_payment,access.programmed.payment,model_programmed_payment,,1,1,1,1
```

groups.xml
```
<?xml version="1.0" encoding="utf-8"?>
<odoo>

    <record id="programmed_payment_category" model="ir.module.category">
            <field name="name">Programmed Payment</field>
            <field name="description">Programmed Payment</field>
            <field name="sequence" eval="90"/>
    </record>

    <record id="programmed_payment_user_group" model="res.groups">
        <field name="name">Programmed Payment User</field>
        <field name="users" eval="[(4, ref('base.user_root')), (4, ref('base.user_admin'))]"/>
        <field name="category_id" ref="programmed_payment_category"/>
        <field name="comment">User</field>
    </record>

    <record id="programmed_payment_admin_group" model="res.groups">
        <field name="name">Programmed Payment Admin</field>
        <field name="users" eval="[(4, ref('base.user_root')), (4, ref('base.user_admin'))]"/>
        <field name="comment">Admin</field>
        <field name="category_id" ref="programmed_payment_category"/>
        <field name="implied_ids" eval="[(4, ref('programmed_payment_user_group'))]"/>
    </record>
</odoo>
```
