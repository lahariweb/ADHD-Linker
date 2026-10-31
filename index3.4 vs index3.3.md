# index.html vs index3.3.html

**Comparison date:** 31 October 2026 (Australia/Melbourne)  
**Scope:** Latest `index.html` produced in this chat versus the uploaded `index3.3.html`. Both source files were inspected; neither HTML file was changed for this comparison.

## Verdict

**Use the latest `index.html` for the requested food, recipe and sync features.** The uploaded file already contains the revised timetable, expanded-only dragging and dotted pause rows. Those are shared features, not new differences between these two files.

The latest file adds the skip-choice popup, Genie-style dialog motion, one-second notifications, safer food-frequency cloud saving and the newer nutrition/portion workflows. Its recipe nutrition is explicitly **estimated**, not independently verified recipe data.

## 1. File facts

| Measure | Uploaded `index3.3.html` | Latest `index.html` |
|---|---:|---:|
| File size | 739,437 bytes | 899,812 bytes |
| Built-in recipe definitions | 133 | 133 |
| Embedded recipe macro profiles | 0 | 133 |
| JavaScript syntax | Pass | Pass |
| Default timetable | Same template | Same template |
| SHA-256 fingerprint | ceb4647503ed816d | ed0ed55ea9e63b28 |

The latest file is 160,375 bytes larger, mainly because it embeds the recipe nutrition profiles and their assumption notes. File size alone is not a performance benchmark.

## 2. Functional differences

| Area | `index3.3.html` | Latest `index.html` | Code evidence |
|---|---|---|---|
| Skip confirmation | Skipping immediately closes the released gap. | Small popup offers Leave time empty, Shift following activities, or Cancel. | requestSkipChoice / confirmEdit |
| Conflict-editor skipping | Closes the gap automatically. | Uses the same empty-or-shift decision before applying a new skip. | applyConflictEditSheet |
| Agenda / parallel activity skip | Marks the item skipped directly. | Asks for the skip choice and routes changes through the conflict/Undo flow. | skipTTActivity |
| Popup animation | No shared Genie animation layer. | Shared Genie-style opening/closing for custom in-app dialogs; reduced-motion fade. Native browser dialogs retain system behaviour. | installPopupGenieMotion |
| Normal / warning notification time | 2.6 seconds normally; 3.6 seconds for warning messages. | 1 second for both. | toast |
| Undo notification time | 8 seconds. | 1 second; Undo history and the header Undo button remain available. | renderUndoToast |
| Food-name cloud-save error | Uses food names as foodFreq object keys; names containing periods or other forbidden characters can reject the write. | Serializes foodFreq under safe food_0-style IDs, with name/count stored as values; decodes older name-keyed maps. | firebaseStatePayload / normalizeFoodFrequency |
| Cloud error handling | A synchronous SDK validation throw can escape the promise catch. | Async cloud writer catches synchronous SDK errors through the save try/catch. | writePlannerStateToCloud / pushToFirebase |
| Manual Sync now | Primarily reads newer remote data; does not retry uploading local data when remote is not newer. | Writes the device state when manually syncing and the remote copy is not newer. | cloudResyncNow |
| Backup import cloud save | Writes the raw state object. | Uses the safe cloud payload writer. | importBackup |
| Vita tab order | Ahara → Training → Supplies → Recipes → Stats. | Ahara → Recipes → Training → Supplies → Stats. | renderArogya |
| Custom-food organisation | One expandable list. | Five buckets: Custom, Morning, Midday, Snack, Dinner. New AI-created custom foods start in Custom. | CUSTOM_FOOD_BUCKETS / renderCustomFoodsList |
| Moving foods between buckets | No bucket-moving control. | Category dropdown inside expanded food details changes the saved bucket; existing meal tags are used as a fallback for older foods. | customFoodBucket / moveCustomFoodBucket |
| Custom-food main row | Name/macros with taller inline editing controls. | Thin Name · Calories · Meal · Add · × row; details and category control expand underneath. | customFoodCard |
| Recipe main row | Recipe details and meal controls occupy more vertical space. | Thin Name · Calories · Meal · Add · × row, with expandable notes and instructions. | recipeCard |
| Existing built-in recipe macros | 133 built-in recipes; no complete embedded macro-profile table. Displayed calories can fall back to a generic recipe-group estimate. | 133 embedded estimated profiles include calories, protein, carbs, fat, sugar, cooked portion weight and ingredient/oil assumptions. | BUILTIN_RECIPE_NUTRITION / recipeKcal |
| User nutrition overrides | No dedicated recipeNutrition override map. | Saved recipeNutrition values take priority over the embedded profiles. | allRecipes |
| New recipe saving | Pasted recipes receive keyword-based macro guesses; link-only bookmarks can save without macros. | Recipe draft opens the AI prompt workflow. It enters the saved collection only after complete nutrition JSON passes validation. | parseRecipeDraft / saveNutritionAIReply |
| Older incomplete custom recipes | Can appear in the normal recipe list. | Kept in a repair section with Get macros with AI; excluded from the normal recipe and pantry-match lists until complete. | renderRecipes / recipeListForMeal |
| Food logging without macros | Unknown values can be replaced with zero and logged. | Requires calories, protein, carbs and fat; missing nutrition routes to AI instead of logging. | hasCompleteFoodMacros / addFoodToMeal |
| Add-to-Ahara popup | No shared portion-quantity popup; adds the food directly. | Exactly three main inputs: one portion size in grams, number of portions, meal type. | openFoodPortionPopup |
| Fractional portions and scaling | No shared entered-weight/portion multiplier. | Allows decimal counts such as 0.1 and 0.2. Multiplier = entered grams × portions ÷ saved macro-basis grams. | confirmFoodPortion |
| Rice/curry serving default | No shared 250 g default rule. | Rice, curry, dal, sambar and related dishes default to 250 g when no weight is supplied. Rice notes identify the chosen rice-cup convention. | defaultFoodPortionGrams |
| Repeated food entries in Ahara | Each logged entry is displayed separately. | Groups the same name within a meal, displays a combined count, and sums calories. Underlying log records remain separate. | groupedMealItems / mealCard |
| Meal portion notes | No structured portion basis on each logged food. | Stores macro-basis weight, portion size, entered count, total eaten grams and nutrition-source notes. | logFoodEntry |
| Delete recipe from library | Built-in recipes have no direct delete control; custom deletion is available in recipe details. | Main-row × removes built-in or custom recipes from the library; Undo restores the change. | deleteRecipeFromLibrary |
| Delete food from library | Custom foods have removal controls; catalog search rows lack delete. | Custom rows have ×; Quick-add rows can hide catalog foods or remove custom foods, with Undo. | removeCustomFoodAt / removeFoodFromLibrary |
| Delete food from a meal | Removes one food-log record. | Grouped meal-row × removes matching food records for that meal; other meals remain unchanged. Undo is supported. | removeMealFoodGroup |
| AI food JSON | Older short nutrition JSON; rounds macro values to whole numbers. | Validates required macros, preserves decimal values, and stores portion/source metadata. Library-only saving is supported. | aiPromptText / parseAiPaste |

## 3. Features present in both files

| Feature | Comparison result |
|---|---|
| Default timetable | The full `DEFAULT_TEMPLATE` values are identical, including wake-up, lunch/bath split, exercise, hobby and evening face care. |
| Daily chore and beauty-care roundups | Both place the selected-day tasks inside the main schedule. |
| Hold-and-drag | Both implement expanded-only schedule dragging without a visible drag handle. |
| Arrows and locks | Both keep these controls within expanded schedule view. |
| Free time and breaks | Both use slim dotted pause rows and exclude them from checklists/progress. |
| Automatic skip reversal | Both restore owned automatic skips when the later completed activity is unchecked. |
| Date preservation | Both preserve the selected calendar date through state loading and use Melbourne date helpers. |
| Theme / neon icons | Both retain the existing Soham theme and schedule icon system. |
| Learning, Business and Journal | Their named functions are unchanged by this comparison, apart from the shared popup styling/motion layer applying when relevant. |
| Brain Dump / Thought Vault | Existing Brain Dump functions are unchanged. The newly requested creation-page redesign was only provided as a prompt, not implemented in either file. |

## 4. Important details and tradeoffs

| Detail | What it means |
|---|---|
| Estimated recipe nutrition | The 133 profiles are AI-generated ingredient-weight models, with explicit oil/water/ingredient assumptions. They were not verified against each linked recipe, package label or measured cooked batch. Actual nutrition can differ. |
| Meaning of `nutritionReviewed` | The seeded profiles set this flag so they can be used. It is an application eligibility flag, not proof of independent nutrition verification. |
| 250 g convention | This is the chosen app default. The rice-cup description is a user convention, not a universal physical cup-to-gram conversion. |
| Calorie text on mobile | The latest CSS hides the small calorie label in thin rows at widths of 650 px or less. The requested full row is therefore clearest on larger screens; nutrition remains available in details/popups. |
| Older Quick-add badges | The latest `quickFoodListHtml` no longer renders the old good/okay/bad tag icon. The old `noteForLoggedFood` helper remains, but the main new Add path no longer calls it. |
| Direct manual macro editing | The latest custom-food row routes nutrition updates through AI JSON instead of showing the previous inline numeric editors and source toggle. |
| AI workflow | It copies a prompt for an external AI chat and accepts pasted JSON. It does not automatically call an AI service or fetch the linked recipe page. |
| Validation limits | Required numbers, positive calories and a portion basis are checked. Validation does not establish whether AI nutrition is factually correct or whether calories perfectly agree with the macros. |
| Library deletion vs meal history | Removing a saved recipe/food does not erase previously logged meals. Deleting the grouped Ahara row removes that food from that meal. |
| Cross-device compatibility | The new decoder understands both old and new food-frequency formats. The older uploaded app does not understand the safe-ID format for frequency ranking; use the same updated HTML across devices. |
| Cloud fix scope | Safe-key serialization specifically covers `foodFreq`; it is not a general sanitizer for every possible field in the database. |

## 5. Verification and evidence

This is a source comparison, not a live deployment or a test of your personal Firebase data. Both inline scripts passed JavaScript syntax parsing. Named functions were compared using their actual JavaScript function text in isolated mocked contexts.

| Named-function comparison | Count |
|---|---:|
| Added in latest | 33 |
| Changed | 30 |
| Unchanged | 609 |
| Removed | 0 |

These counts exclude CSS and embedded data changes. Keeping a function does not mean its surrounding UI or all its callers are unchanged.

### Source locations

Line numbers refer to the exact files compared and may move after later edits.

| Function | `index3.3.html` line | `index.html` line |
|---|---:|---:|
| `pushToFirebase` | 1792 | 1822 |
| `cloudResyncNow` | 1751 | 1779 |
| `toast` | 1860 | 1892 |
| `renderUndoToast` | 1869 | 1901 |
| `confirmEdit` | 3715 | 3766 |
| `renderArogya` | 6493 | 6547 |
| `allRecipes` | 6524 | 8708 |
| `openFoodPortionPopup` | — | 8771 |
| `confirmFoodPortion` | — | 8780 |
| `validateNutritionReply` | — | 8751 |
| `parseRecipeDraft` | 7364 | 9578 |
| `recipeCard` | 7430 | 9639 |
| `customFoodCard` | — | 9277 |
| `renderCustomFoodsList` | 7034 | 9282 |
| `installPopupGenieMotion` | — | 12103 |
| `renderBrainDump` | 4003 | 4057 |

## 6. Practical choice

| Goal | Choice |
|---|---|
| Keep the current requested food and portion features | Latest `index.html` |
| Keep the earlier simpler food workflow | Uploaded `index3.3.html`, while accepting that it lacks the newer fixes |
| Preserve the timetable only | Both have the same default timetable |
| Build the new Brain Dump creation page | Still needs implementation; neither file contains that redesign |

**Recommended next refinements:** keep calories visible on mobile and decide whether to restore the older Quick-add quality badges/nudges. No such changes were applied during this comparison.
