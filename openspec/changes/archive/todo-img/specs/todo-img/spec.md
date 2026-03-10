---
featureId: todo-img
title: Todo 圖片附加功能 - 變更規格
status: draft
owner: PM
lastUpdated: 2026-03-10
changeType: new-feature
---

# Delta Spec: Todo Image Attachment Feature

## ADDED Requirements

### ADDED: Image Upload Capability
Users can upload a single image when creating or editing a todo item.

**Constraints:**
- Supported formats: JPG, PNG, WebP
- Maximum file size: 5 MB
- Image validation required (format + size check)
- Upload progress feedback must be provided

#### Scenario: Upload image during todo creation
```
Given user is on the todo creation page
When user fills in the todo title
And user clicks "Add Image" button
And user selects a JPG file (3 MB)
And system displays upload progress
Then upload should complete successfully
And image preview should be displayed
And "Create" button should remain enabled
```

#### Scenario: Reject oversized image
```
Given user is uploading an image
When user selects an 8 MB file
And system validates file size
Then error message should display: "File too large, please select an image under 5 MB"
And upload should be cancelled
And todo should remain unchanged
```

#### Scenario: Reject unsupported format
```
Given user is uploading an image
When user selects a GIF file
And system validates file format
Then error message should display: "Format not supported. Please select JPG, PNG, or WebP"
And upload should be cancelled
```

---

### ADDED: Image Display in Todo List
Todo items with images show a thumbnail in the list view.

**Constraints:**
- Thumbnail size: ~40x40 px (adjustable based on UI design)
- Thumbnail should not disrupt list layout
- Todos without images should not show empty image area

#### Scenario: Display image thumbnail in list
```
Given a todo with an attached image exists
When user views the todo list
Then the todo item should display the image thumbnail
And thumbnail size should be consistent with other todos
```

---

### ADDED: Image Display in Detail View
Todo detail page displays the full-size image.

**Constraints:**
- Image should scale to container width
- Aspect ratio must be maintained
- Image should be responsive on different screen sizes

#### Scenario: View full image in detail page
```
Given a todo with an attached image exists
When user clicks on the todo
Then detail page should open
And full-size image should be displayed
And image should be responsive to screen width
And editing options should be available below image
```

---

### ADDED: Image Replacement
Users can replace an image when editing a todo.

**Constraints:**
- Only one image per todo (old image must be removed before new one is added)
- Same validation rules apply (format, size)
- Replacement should be atomic (either fully replaced or unchanged)

#### Scenario: Replace image in edit mode
```
Given a todo with an attached image in edit mode
When user clicks "Replace Image" button
And user selects a new PNG file (2 MB)
And system completes upload
Then old image should be removed
And new image should be displayed
And changes should be saved when user confirms
```

---

### ADDED: Image Deletion
Users can remove an image from a todo.

**Constraints:**
- Deletion should require confirmation
- Deletion should be immediate and irreversible
- Empty image area should not be shown after deletion

#### Scenario: Delete image with confirmation
```
Given a todo with an attached image in edit mode
When user clicks "Delete Image" button
Then confirmation dialog should appear: "Are you sure you want to delete this image?"
When user clicks "Confirm"
Then image should be immediately deleted
And both list and detail views should update
And no empty image area should be shown
```

#### Scenario: Cancel image deletion
```
Given confirmation dialog is open for image deletion
When user clicks "Cancel"
Then image should remain unchanged
And dialog should close
```

---

### ADDED: Image Persistence
Images are stored persistently and removed when todo is deleted.

**Constraints:**
- Images must survive app restart
- Images must be cleared when their parent todo is deleted
- No orphaned images should remain in storage

#### Scenario: Image persists after todo creation
```
Given a todo with image has been created
When user closes and reopens the app
And user navigates to the todo
Then image should still be displayed
And image should load without errors
```

#### Scenario: Image cleanup on todo deletion
```
Given a todo with an attached image exists
When user deletes the todo
Then image should be permanently removed from storage
And no orphaned image files should remain
```
