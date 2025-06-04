# BL Form Logic Library Documentation

**Version 0.1.0** | For Webflow Developers

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

| Operator | When to Use | Example Value |
|:---------|:------------|:---------------|
| `equals` | Exact match (default) | `yes` |
| `not-equals` | Doesn't match | `no` |
| `contains` | Contains text | `john` (matches "John Doe") |
| `greater-than` | Number comparison | `18` |
| `less-than` | Number comparison | `65` |
| `empty` | Field is empty | (leave value empty) |
| `not-empty` | Field has any value | (leave value empty) |
| `includes` | Any of multiple values | `red,blue,green` |

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

#### Setting Up Validation

1. **Select your form fields**
2. **In Field Settings**: Check "Required" if needed
3. **For numbers**: Set Min/Max values
4. **For text**: Set character limits
5. **For custom patterns**: Add custom attribute `pattern` with your regex

**The library handles the rest automatically!**

---

## 🔧 API Reference

### Configuration Options

```javascript
// Optional: Custom configuration in Footer Code (after the main script)
window.bl = new BL({
  hiddenClass: 'bl-hidden',        // CSS class for hidden fields
  animationDuration: 300,          // Animation speed in milliseconds
  clearHiddenValues: true,         // Clear values when fields are hidden
  validateOnInput: true,           // Real-time validation
  debug: false                     // Enable console logging
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
document.addEventListener('bl:step:changed', function(e) {
  console.log('Now on step:', e.detail.currentStep + 1);
  console.log('Total steps:', e.detail.totalSteps);

  // Example: Update a custom step counter
  document.querySelector('.step-counter').textContent = 
    `Step ${e.detail.currentStep + 1} of ${e.detail.totalSteps}`;
});

// When conditional fields show/hide
document.addEventListener('bl:field:shown', function(e) {
  console.log('Field shown:', e.detail.element);
});

document.addEventListener('bl:field:hidden', function(e) {
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

| Class | Applied To | Purpose |
|:------|:-----------|:--------|
| `.bl-hidden` | Hidden conditional elements | Hides elements |
| `.bl-step-active` | Current step | Shows current step |
| `.bl-button-disabled` | Invalid navigation buttons | Disables buttons |
| `.active-step` | Current step indicators | Highlights active indicators |

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
- [ ] Form has `data-bl-form-element="multistep"` (for multi-step)
- [ ] Each step div has `data-bl-form-element="step"`
- [ ] Navigation buttons have correct attributes
- [ ] Conditional elements have `data-bl-condition-field` and `data-bl-condition-value`
- [ ] All trigger fields have proper **Name** values in Field Settings
- [ ] Step indicators have container and element attributes
- [ ] Required fields are marked as Required in Webflow

**The BL library works entirely through Webflow's visual interface - no coding required!**

