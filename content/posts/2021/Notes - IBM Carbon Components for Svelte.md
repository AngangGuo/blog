---
title: "Notes - IBM Carbon Components for Svelte"
date: 2021-03-15T13:00:51-07:00
categories:
- Tech
- Web
tags:
- Svelte
- Framework
draft: false
---

## IBM Carbon Components - Svelte

### DatePicker
```sveltehtml
<script>
  import { DatePicker, DatePickerInput } from "carbon-components-svelte";
</script>

<DatePicker datePickerType="single" dateFormat="m/d/Y">
  <DatePickerInput labelText="Schedule a meeting" placeholder="mm/dd/yyyy" />
</DatePicker>
```

* When set `datePickerType` to `single` or `range`, the calendar will show out
* `dateFormat` use only one character for day, month and year.
  e.g.  March 15, 2021 - `m/d/Y`(`03/15/2021`) or `Y-m-d`(`2021-03-15`)


### Reference
* [IBM Carbon Design System](https://www.carbondesignsystem.com/)
* [Carbon-Svelte Storybook](https://carbon-svelte.vercel.app/)
* [Github - Carbon Svelte Components](https://github.com/IBM/carbon-components-svelte)

