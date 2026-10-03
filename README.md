# ServiceNow Incident Management Client Scripts

## Project Overview
Implementation of custom Client Scripts on the Incident table in ServiceNow to enforce business rules and automate field updates.

---

## 1. Prevent State Change via List Edit
* **Table:** Incident
* **Type:** onCellEdit
* **Field name:** State

```javascript
function onCellEdit(sysIDs, table, oldValues, newValue, callback) {
    alert('State cannot be updated using list editing. Please open the Incident.');
    callback(false);
}
```

## 2. Prevent Save if Assigned To Missing
* **Table:** Incident
* **Type:** `onSubmit`

```javascript
function onSubmit() {
    if (g_form.getValue('impact') == '1' && g_form.getValue('assigned_to') == '') {
        g_form.showErrorBox('assigned_to', 'Assigned To is mandatory for High impact incidents.');
        return false;
    }
}
```

## 3. Auto Set Urgency for High Impact
* **Table:** Incident
* **Type:** `onChange`
* **Field name:** Impact

```javascript
function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading || newValue == '') {
        return;
    }

    if (newValue == '1') {
        g_form.setValue('urgency', '1');
        g_form.addInfoMessage('Urgency set to High for High impact incident.');
    }
}
```

  
