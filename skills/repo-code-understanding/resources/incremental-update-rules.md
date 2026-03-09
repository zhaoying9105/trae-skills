# Incremental Analysis Update Rules

Follow these rules when updating documentation based on new MR/commit changes:

## 1. Change Impact Assessment
For each change, determine the scope of impact:
- **Trivial change** (cosmetic, comment, formatting): No documentation update needed
- **Minor change** (bug fix, small feature tweak): Update relevant section in affected module docs
- **Significant change** (new feature, API change, refactoring): Full reanalysis of affected module
- **Architectural change** (new module, dependency change, layer restructuring): Update architecture overview + affected modules

## 2. Module Update Triggers
Reanalyze a module if:
- More than 20% of its files are modified
- The public API or interface changes
- Core business logic is significantly altered
- Dependencies on other modules change
- New submodules are added to it

## 3. Cross-Module Updates
If changes affect multiple modules:
- First update all directly affected modules
- Check if dependency graphs need updating
- Update architecture documentation if component relationships change
- Scan for modules that depend on the changed components and update their documentation if needed

## 4. Versioning and Changelog
- Track all incremental updates in changelog.md
- Include commit/MR ID, date, and summary of changes
- Link to updated documentation sections
- Maintain a version number for the documentation set

## 5. Conflict Resolution
If automated analysis is uncertain:
- Flag the section for human review
- Keep both old and new interpretations with notes
- Highlight ambiguous changes in the output
- Don't overwrite existing documentation with low-confidence analysis

## 6. Performance Optimizations
- Cache analysis results for unchanged modules
- Only process diffs instead of full file contents when possible
- Use hash checks to quickly identify unchanged files
- Skip reanalysis for modules that haven't been modified
