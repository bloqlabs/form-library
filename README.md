# BL Form Logic Library Documentation

**Version 0.2.1** | For Webflow Developers

A powerful JavaScript library that adds conditional logic, multi-step navigation, and advanced validation to your Webflow forms without any coding required.

---

## 🚀 Getting Started

### Installation

1. **Add the Script**: In your Webflow project, go to **Project Settings** > **Custom Code** > **Footer Code**
2. **Paste the entire BL library script** (you should have received this separately)
3. **Publish your site** - The library automatically initializes when your page loads

### Your First Conditional Field

Let's create a simple example where a text field appears only when "Yes" is selected:

1. **Create your trigger field**: Add a Select element and set its **Name** to `show_details`
2. **Add options**: "Select an option", "Yes", "No" (make sure "Yes" has value `yes`)
3. **Create a Div** around the field you want to show/hide
4. **Add these attributes** to the Div in Element Settings:
   - **Attribute**: `data-bl-condition-field` **Value**: `show_details`
   - **Attribute**: `data-bl-condition-value` **Value**: `yes`

**What happens**: The div (and everything inside) automatically hides until "Yes" is selected.

---

## 📋 Features

### 1. Multi-Step Forms

Transform any form into a step-by-step wizard with automatic navigation.

#### Setup Your Form

1. **Select your Form element**
2. **Add attribute**: `data-bl-form-element` **Value**: `multistep`

#### Create Steps

1. **Add Div elements** inside your form for each step
2. **For each step Div, add attribute**: `data-bl-form-element` **Value**: `step`
3. **Put your form fields** inside each step Div

#### Add Navigation Buttons

Create three button elements and add these attributes:

**Back Button:**

- **Add to Button or Div containing button**: `data-bl-form-element` **Value**: `back`

**Next Button:**

- **Add to Button or Div containing button**: `data-bl-form-element` **Value**: `next`

**Submit Button:**

- **Add to Button or Div containing button**: `data-bl-form-element` **Value**: `submit`

#### How Navigation Works

- **Back button**: Automatically hidden on first step
- **Next button**: Hidden on last step, disabled if validation fails
- **Submit button**: Only appears on last step, disabled if validation fails

#### Validation Modes

**Strict Mode (Default):**

- Buttons are disabled when current step has validation errors
- Users must fix errors before proceeding
- No error messages shown - buttons simply won't work

**Permissive Mode:**

- Buttons are never disabled - users can always click
- When validation fails, error messages appear inline
- Better for forms where users want to see all steps first

**To enable Permissive Mode:**

1. **Select your Form element**
2. **Add attribute**: `data-bl-validation-mode` **Value**: `permissive`

```html
<!-- Example: Permissive validation with custom error styling -->
<form
	data-bl-form-element="multistep"
	data-bl-validation-mode="permissive"
	data-bl-error-class="my-custom-error"></form>
```

**Note:** You can use either `data-bl-form-element="multistep"` or `data-bl-form-element="form"` - both work the same way.

#### Error Messages in Permissive Mode

**Automatic Error Containers:**
The library creates error messages automatically, or you can create dedicated containers:

```html
<div class="field-wrapper">
	<input name="email" type="email" required />
	<div data-bl-error-for="email" class="error-message"></div>
</div>
```

**Custom Error Messages:**
Override default messages per field:

```html
<input
	name="email"
	type="email"
	required
	data-bl-error-message="Please enter your email address" />
```

#### File Upload Validation

- Use the native `accept` attribute on your file input to restrict the allowed file types. The BL library double-checks that the uploaded file matches the accepted extensions or MIME types.
- Add the attribute `data-bl-max-file-size` to set a per-field upload limit (value in kilobytes). Example: `data-bl-max-file-size="10240"` restricts uploads to 10 MB.
- Users receive inline errors when a file is larger than allowed or does not match the accepted formats.

### 2. Step Indicators

Visual progress indicators that automatically match your form steps.

#### Auto-Duplicate Setup (Recommended)

1. **Create a Div** for your indicator container
2. **Add attribute**: `data-bl-form-element` **Value**: `step-indicator` (for progress bar) or `step-indicator-single` (for current step only)
3. **Inside the container, add ONE element** (Div, span, etc.) for your indicator
4. **Add attribute to this element**: `data-bl-form-element` **Value**: `step-indicator-element`

**The library automatically duplicates this single element** to match your number of steps!

#### Indicator Types

**Progress Indicator** (`step-indicator`):

- Shows all completed steps as active
- Perfect for progress bars

**Current Only** (`step-indicator-single`):

- Only highlights the current step
- Great for "Step X of Y" displays

#### Styling Your Indicators

Add a class to your step indicator elements and style the `.active-step` state:

```css
.my-step-dot {
	width: 20px;
	height: 20px;
	background: #ddd;
	border-radius: 50%;
	transition: background 0.3s ease;
}

.my-step-dot.active-step {
	background: #007bff;
}
```

### 3. Conditional Fields

Show or hide any element based on form inputs.

#### Basic Setup

1. **Select the element** you want to show/hide (Div, form field, etc.)
2. **Add these attributes**:
   - `data-bl-condition-field` **Value**: `name_of_trigger_field`
   - `data-bl-condition-value` **Value**: `value_to_match`

#### Advanced Conditions

**Use different operators** by adding:

- `data-bl-condition-operator` **Value**: `operator_name`

**Multiple conditions** (all must be true):

- `data-bl-condition-field-2` **Value**: `second_field_name`
- `data-bl-condition-value-2` **Value**: `second_value`
- Continue with `-3`, `-4`, etc. for more conditions

#### Available Operators

| Operator       | When to Use            | Example Value               |
| :------------- | :--------------------- | :-------------------------- |
| `equals`       | Exact match (default)  | `yes`                       |
| `not-equals`   | Doesn't match          | `no`                        |
| `contains`     | Contains text          | `john` (matches "John Doe") |
| `greater-than` | Number comparison      | `18`                        |
| `less-than`    | Number comparison      | `65`                        |
| `empty`        | Field is empty         | (leave value empty)         |
| `not-empty`    | Field has any value    | (leave value empty)         |
| `includes`     | Any of multiple values | `red,blue,green`            |

### 4. Form Validation

Built-in validation using Webflow's native form settings.

#### Automatic Validation

The library automatically validates:

- **Required fields** (set in Webflow field settings)
- **Email fields** (Email input type)
- **Phone fields** (Phone input type)
- **Number fields** with min/max values
- **Text fields** with character limits
- **Custom patterns** (add `pattern` attribute)
- **Email domain blocking** (block specific email domains)

#### Setting Up Validation

1. **Select your form fields**
2. **In Field Settings**: Check "Required" if needed
3. **For numbers**: Set Min/Max values
4. **For text**: Set character limits
5. **For custom patterns**: Add custom attribute `pattern` with your regex

**The library handles the rest automatically!**

#### Email Domain Blocking

Block email addresses from specific domains (e.g., prevent personal emails in business forms).

**Setup:**

Add the `data-bl-blocked-domains` attribute to either:
- **Individual email field** (applies to that field only)
- **Form element** (applies to all email fields in the form)

**Examples:**

```html
<!-- Block specific domains -->
<input 
  type="email" 
  name="business_email"
  data-bl-blocked-domains="gmail.com,yahoo.com,hotmail.com">

<!-- Use default blocked domains (gmail.com, msn.com, gmx.com) -->
<input 
  type="email" 
  name="work_email"
  data-bl-blocked-domains="">

<!-- Apply to entire form -->
<form 
  data-bl-form-element="multistep"
  data-bl-blocked-domains="gmail.com,outlook.com">
```

**How it works:**
- Validates email domain on blur and during form submission
- Shows localized error messages (supports all 5 languages)
- Field-level attribute overrides form-level setting
- Empty attribute (`data-bl-blocked-domains=""`) uses default blocked domains
- Comma-separated list for multiple domains

### 5. Multi-Language Support

Perfect for international websites - the library automatically detects your visitor's language and shows error messages in their native language.

#### Supported Languages (Built-in)

- **English** (en) - Default
- **Spanish** (es) - Español
- **French** (fr) - Français
- **Italian** (it) - Italiano
- **German** (de) - Deutsch

#### Automatic Language Detection

The library automatically detects language in this order:

1. **Form attribute** `data-bl-language` (highest priority)
2. **HTML lang attribute** on your page
3. **Browser language** setting
4. **English fallback**

#### Setting Your Language

**Option 1: Set on Form (Recommended)**

```html
<form
	data-bl-form-element="multistep"
	data-bl-validation-mode="permissive"
	data-bl-language="es"></form>
```

**Note:** You can use `data-bl-form-element="form"` instead of `"multistep"` - both work identically.

**Option 2: Set on HTML element**

```html
<html lang="es"></html>
```

#### Custom Error Messages

**Single Language:**

```html
<input
	name="email"
	type="email"
	required
	data-bl-error-message="Tu email es requerido" />
```

**Multiple Languages:**

```html
<input
	name="email"
	type="email"
	required
	data-bl-error-message-en="Email is required"
	data-bl-error-message-es="El email es requerido"
	data-bl-error-message-fr="L'email est requis" />
```

#### Webflow CMS Integration

For dynamic multi-language sites using Webflow CMS:

```html
<!-- Use CMS fields for language and translations -->
<form
	data-bl-form-element="multistep"
	data-bl-language='{{wf {"path":"language-code"} }}'
	data-bl-validation-mode="permissive">
	<input
		name="email"
		data-bl-error-message='{{wf {"path":"email-error-text"} }}' />
</form>
```

**Note:** You can use `data-bl-form-element="form"` instead of `"multistep"` - both are equivalent.

#### Built-in Error Messages

The library includes professional translations for all validation errors:

| Error Type     | English                                  | Spanish                                | French                                    |
| :------------- | :--------------------------------------- | :------------------------------------- | :---------------------------------------- |
| Required       | "This field is required"                 | "Este campo es obligatorio"           | "Ce champ est obligatoire"               |
| Email          | "Please enter a valid email"             | "Ingrese un email válido"             | "Veuillez saisir un email valide"        |
| Phone          | "Please enter a valid phone number"      | "Ingrese un teléfono válido"          | "Veuillez saisir un numéro valide"       |
| Blocked Domain | "Email from this domain is not allowed"  | "No se permite email de este dominio" | "Email de ce domaine n'est pas autorisé" |

_All 5 languages have complete translation sets for every error type._

---

## 🔧 API Reference

### Configuration Options

```javascript
// Optional: Custom configuration in Footer Code (after the main script)
window.bl = new BL({
	hiddenClass: 'bl-hidden', // CSS class for hidden fields
	animationDuration: 300, // Animation speed in milliseconds
	clearHiddenValues: true, // Clear values when fields are hidden
	validateOnInput: true, // Real-time validation
	validationMode: 'strict', // 'strict' or 'permissive'
	showInlineErrors: true, // Show error messages (permissive mode)
	errorMessageClass: 'bl-error-message', // CSS class for error messages
	fieldErrorClass: 'bl-field-error', // CSS class for invalid fields
	debug: false, // Enable console logging
});
```

### Methods

#### Navigation Methods

```javascript
// Go to specific step (0-based index)
bl.goToStep(2);

// Get current step number (0-based)
const currentStep = bl.getCurrentStep();

// Get total number of steps
const totalSteps = bl.getTotalSteps();

// Check if all steps are valid
const allValid = bl.validateAllSteps();

// Refresh the library (after dynamic changes)
bl.refresh();
```

#### Translation Methods

```javascript
// Add custom translations for a language
bl.addTranslation('pt', {
	required: 'Este campo é obrigatório',
	email: 'Digite um email válido',
});

// Change language programmatically
bl.setLanguage('fr');

// Get current language
const currentLang = bl.getLanguage();
```

#### Utility Methods

```javascript
// Get debug information
const info = bl.getDebugInfo();
console.log(info);

// Destroy the instance
bl.destroy();
```

### Custom Events

Listen for events in your custom code:

```javascript
// When user changes steps
document.addEventListener('bl:step:changed', function (e) {
	console.log('Now on step:', e.detail.currentStep + 1);
	console.log('Total steps:', e.detail.totalSteps);

	// Example: Update a custom step counter
	document.querySelector('.step-counter').textContent = `Step ${
		e.detail.currentStep + 1
	} of ${e.detail.totalSteps}`;
});

// When conditional fields show/hide
document.addEventListener('bl:field:shown', function (e) {
	console.log('Field shown:', e.detail.element);
});

document.addEventListener('bl:field:hidden', function (e) {
	console.log('Field hidden:', e.detail.element);
});
```

---

## 📖 Real Examples

### Multi-Step Contact Form

**Form Setup:**

1. **Form element**: Add attribute `data-bl-form-element="multistep"`

**Step 1 - Contact Info:**

1. **Div around fields**: Add attribute `data-bl-form-element="step"`
2. **Add fields**: Name (required), Email (required), Phone (required)

**Step 2 - Service Selection:**

1. **Another Div**: Add attribute `data-bl-form-element="step"`
2. **Select field**: Name it `service`, add options like "Web Design", "Development"
3. **Conditional Div**: For extra questions when "Web Design" is selected
   - Add attribute `data-bl-condition-field="service"`
   - Add attribute `data-bl-condition-value="web-design"`

**Progress Indicator:**

1. **Div container**: Add attribute `data-bl-form-element="step-indicator"`
2. **Single dot inside**: Add attribute `data-bl-form-element="step-indicator-element"`
3. **Style with CSS class** (the library creates copies automatically)

**Navigation:**

1. **Back button**: Add attribute `data-bl-form-element="back"`
2. **Next button**: Add attribute `data-bl-form-element="next"`
3. **Submit button**: Add attribute `data-bl-form-element="submit"`

### Survey with Conditional Logic

**Age Question:**

1. **Select field**: Name it `age_range`, add options

**Adult-Only Question:**

1. **Div around question**:
   - Add attribute `data-bl-condition-field="age_range"`
   - Add attribute `data-bl-condition-value="under-18"`
   - Add attribute `data-bl-condition-operator="not-equals"`

**Multiple Choice Conditions:**

1. **Div for specific ages**:
   - Add attribute `data-bl-condition-field="age_range"`
   - Add attribute `data-bl-condition-value="18-25,26-35"`
   - Add attribute `data-bl-condition-operator="includes"`

---

## 🎨 Styling

### Automatic CSS Classes

The library adds these classes automatically:

| Class                 | Applied To                  | Purpose                      |
| :-------------------- | :-------------------------- | :--------------------------- |
| `.bl-hidden`          | Hidden conditional elements | Hides elements               |
| `.bl-step-active`     | Current step                | Shows current step           |
| `.bl-button-disabled` | Invalid navigation buttons  | Disables buttons             |
| `.active-step`        | Current step indicators     | Highlights active indicators |
| `.bl-error-message`   | Error message containers    | Styles error text            |
| `.bl-field-error`     | Invalid form fields         | Highlights invalid fields    |

### Example Styling

```css
/* Smooth step transitions */
.bl-step {
	opacity: 0;
	transition: opacity 0.3s ease;
}

.bl-step.bl-step-active {
	opacity: 1;
}

/* Step indicator dots */
.step-dot {
	width: 12px;
	height: 12px;
	background: #ddd;
	border-radius: 50%;
	margin: 0 4px;
	transition: all 0.3s ease;
}

.step-dot.active-step {
	background: #007bff;
	transform: scale(1.2);
}

/* Disabled button styling */
.bl-button-disabled {
	opacity: 0.5;
	cursor: not-allowed;
}

/* Error message styling */
.bl-error-message {
	color: #dc3545;
	font-size: 0.875em;
	margin-top: 0.25rem;
	display: block;
}

.bl-field-error {
	border-color: #dc3545 !important;
	box-shadow: 0 0 0 0.2rem rgba(220, 53, 69, 0.25);
}

/* Custom error container */
.error-message {
	background: #f8d7da;
	color: #721c24;
	padding: 0.5rem;
	border-radius: 0.25rem;
	margin-top: 0.5rem;
	border: 1px solid #f5c6cb;
}
```

---

## 🐛 Troubleshooting

### Common Issues

**❌ Conditional fields not working**

- Check that your trigger field has the correct **Name** in Field Settings
- Verify the **Name** matches your `data-bl-condition-field` value exactly
- Make sure your option values match `data-bl-condition-value`

**❌ Multi-step not working**

- Ensure your form has the `data-bl-form-element="multistep"` attribute
- Check that each step div has `data-bl-form-element="step"`
- Verify navigation buttons have the correct attributes

**❌ Step indicators not updating**

- Confirm the container has `data-bl-form-element="step-indicator"`
- Check that your indicator element has `data-bl-form-element="step-indicator-element"`

**❌ Error messages not showing (Permissive mode)**

- Verify you have `data-bl-validation-mode="permissive"` on your form
- Check that fields have proper validation attributes (required, type, etc.)
- Ensure error containers exist or library can create them
- Look for custom error message attributes like `data-bl-error-message`

**❌ Wrong language showing**

- Check your `data-bl-language` attribute on the form
- Verify your HTML `lang` attribute if not using form attribute
- Supported languages: en, es, fr, it, de

### Debug Mode

Add this to your Footer Code (after the main script) to see detailed logs:

```javascript
window.bl = new BL({ debug: true });
```

Then check your browser's Console (F12) for helpful information.

---

## ✅ Quick Checklist

**Before Publishing:**

- [ ] Script added to Project Settings > Custom Code > Footer Code
- [ ] Form has `data-bl-form-element="multistep"` or `data-bl-form-element="form"`
- [ ] Each step div has `data-bl-form-element="step"`
- [ ] Navigation buttons have correct attributes
- [ ] Conditional elements have `data-bl-condition-field` and `data-bl-condition-value`
- [ ] All trigger fields have proper **Name** values in Field Settings
- [ ] Step indicators have container and element attributes
- [ ] Required fields are marked as Required in Webflow

**The BL library works entirely through Webflow's visual interface - no coding required!**
