# BAC Tracking Delegate Test Generation Instructions

## Overview

This instruction file guides the generation of unit test classes for BAC (Business Analytics Collection) tracking delegates. The tests focus on validating tracking functionality, object tracking workflow, attribute management, and proper collection category handling for Windchill objects.

**Target Pattern**: `BAC{ObjectType}TrackingDelegateTest.java`
**Example**: `BACWTChangeOrder2TrackingDelegateTest.java`

## Directory Configuration

- **Package Structure**: `wt.{domain}.bac`
- **Test Location**: Same package as the tracking delegate class
- **Naming Convention**: `{TrackingDelegateClassName}Test.java`
- `object.json` = `c:/FINAL/.github/object.json`

## Read Information from object.json
- **Object Type**: Extract the Windchill object type for tracking
- **Collect all fields of Object**: Extract all fields related to the object type for test generation

## Delegate Access

BAC tracking delegates typically implement these core interfaces:
- Collection category identification via `getCollectionCategory()`
- Object tracking via `trackObject(Persistable)`
- Attribute management via `setTrackingAttributes(BACTrackingRecord, Persistable)`
- Tracking workflow coordination
- Collection processing and validation

## Analysis Workflow

1. **Identify Object Type**: Determine the Windchill object type for tracking
2. **Validate Collection Category**: Ensure proper CollectionCategory enum usage
3. **Test Object Tracking**: Verify tracking of objects in the system
4. **Test Attribute Setting**: Validate attribute extraction and setting on tracking records
5. **Test Collection Processing**: Handle null, empty, and populated collections
6. **Verify Inheritance**: Confirm proper inheritance from BACDefaultTrackingDelegate

## Object Dependencies and Attributes Extraction

### Core Object Components:
- Primary Windchill object (e.g., WTChangeOrder2)
- Master object (e.g., WTChangeOrder2Master)
- Collection containers (WTHashSet, WTSet)
- BACTrackingRecord for tracking objects

### Tracking Patterns:
- Object tracking and validation
- Attribute extraction from persistable objects
- Collection category management
- Lifecycle event handling

## Dependencies Identification

### From object.json:
```json
{
  "dependencies": [
    "wt.fc.PersistenceHelper",
    "wt.util.WTException",
    "com.ptc.windchill.bac.tracking.BACTrackingRecord"
  ]
}
```

## Test Class Structure Template

```java
package wt.{domain}.bac;

import static org.junit.Assert.*;
import org.junit.Before;
import org.junit.Test;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import static org.mockito.Mockito.*;

import wt.fc.PersistenceHelper;
import wt.fc.Persistable;
import wt.util.WTException;
import wt.util.WTHashSet;
import wt.util.WTSet;
import com.ptc.windchill.bac.CollectionCategory;
import com.ptc.windchill.bac.tracking.BACTrackingRecord;

/**
 * Test class for BAC{ObjectType}TrackingDelegate.
 * 
 * Tests the tracking delegate functionality including:
 * - Collection category identification
 * - Object tracking workflow
 * - Attribute management for tracking records
 * - Collection processing and validation
 * 
 * @author Generated
 * @version 1.0
 */
public class BAC{ObjectType}TrackingDelegateTest {
    
    private BAC{ObjectType}TrackingDelegate delegate;
    
    @Mock
    private {ObjectType} mockObject;
    
    @Mock
    private {ObjectType}Master mockMaster;
    
    @Mock
    private BACTrackingRecord mockTrackingRecord;
    
    @Before
    public void setUp() throws Exception {
        MockitoAnnotations.initMocks(this);
        delegate = new BAC{ObjectType}TrackingDelegate();
        
        // Setup mock object relationships
        when(mockObject.getMaster()).thenReturn(mockMaster);
    }
    
    /**
     * Test that the delegate returns the correct collection category.
     */
    @Test
    public void testGetCollectionCategory() {
        CollectionCategory category = delegate.getCollectionCategory();
        assertNotNull("Collection category should not be null", category);
        assertEquals("Collection category should match object type", 
                     CollectionCategory.{ObjectType}, category);
    }
    
    /**
     * Test tracking a valid object.
     */
    @Test
    public void testTrackObject_ValidObject() throws Exception {
        // Given
        when(mockObject.getPersistInfo()).thenReturn(mock(wt.fc.PersistInfo.class));
        
        // When
        boolean result = delegate.trackObject(mockObject);
        
        // Then
        assertTrue("Should track valid object", result);
    }
    
    /**
     * Test tracking a null object.
     */
    @Test
    public void testTrackObject_NullObject() throws Exception {
        // When
        boolean result = delegate.trackObject(null);
        
        // Then
        assertFalse("Should not track null object", result);
    }
    
    /**
     * Test setting tracking attributes on a tracking record.
     */
    @Test
    public void testSetTrackingAttributes() throws Exception {
        // Given
        when(mockObject.getName()).thenReturn("TestObject");
        when(mockObject.getNumber()).thenReturn("12345");
        
        // When
        delegate.setTrackingAttributes(mockTrackingRecord, mockObject);
        
        // Then
        verify(mockTrackingRecord, atLeastOnce()).setAttribute(anyString(), any());
    }
    
    /**
     * Test setting tracking attributes with null tracking record.
     */
    @Test(expected = IllegalArgumentException.class)
    public void testSetTrackingAttributes_NullRecord() throws Exception {
        // When
        delegate.setTrackingAttributes(null, mockObject);
        
        // Then - exception expected
    }
    
    /**
     * Test setting tracking attributes with null object.
     */
    @Test(expected = IllegalArgumentException.class)
    public void testSetTrackingAttributes_NullObject() throws Exception {
        // When
        delegate.setTrackingAttributes(mockTrackingRecord, null);
        
        // Then - exception expected
    }
    
    /**
     * Test tracking multiple objects in a collection.
     */
    @Test
    public void testTrackObjects_Collection() throws Exception {
        // Given
        WTSet objects = new WTHashSet();
        objects.add(mockObject);
        
        {ObjectType} mockObject2 = mock({ObjectType}.class);
        when(mockObject2.getPersistInfo()).thenReturn(mock(wt.fc.PersistInfo.class));
        objects.add(mockObject2);
        
        // When
        int trackedCount = delegate.trackObjects(objects);
        
        // Then
        assertEquals("Should track all valid objects", 2, trackedCount);
    }
    
    /**
     * Test tracking an empty collection.
     */
    @Test
    public void testTrackObjects_EmptyCollection() throws Exception {
        // Given
        WTSet objects = new WTHashSet();
        
        // When
        int trackedCount = delegate.trackObjects(objects);
        
        // Then
        assertEquals("Should track zero objects from empty collection", 0, trackedCount);
    }
    
    /**
     * Test tracking a null collection.
     */
    @Test
    public void testTrackObjects_NullCollection() throws Exception {
        // When
        int trackedCount = delegate.trackObjects(null);
        
        // Then
        assertEquals("Should track zero objects from null collection", 0, trackedCount);
    }
    
    /**
     * Test tracking workflow integration.
     */
    @Test
    public void testTrackingWorkflow() throws Exception {
        // Given
        when(mockObject.getPersistInfo()).thenReturn(mock(wt.fc.PersistInfo.class));
        when(mockObject.getName()).thenReturn("TestWorkflow");
        
        // When
        boolean tracked = delegate.trackObject(mockObject);
        BACTrackingRecord record = delegate.createTrackingRecord(mockObject);
        delegate.setTrackingAttributes(record, mockObject);
        
        // Then
        assertTrue("Should successfully track object", tracked);
        assertNotNull("Should create tracking record", record);
    }
}
```

## Test Generation Requirements

### 1. Collection Category Tests
- Verify correct category is returned
- Ensure category is not null
- Validate category matches object type

### 2. Object Tracking Tests
- Test tracking valid objects
- Test tracking null objects
- Test tracking invalid objects
- Verify tracking workflow

### 3. Attribute Management Tests
- Test setting attributes on tracking records
- Test null tracking record handling
- Test null object handling
- Verify attribute extraction

### 4. Collection Processing Tests
- Test tracking multiple objects
- Test empty collection handling
- Test null collection handling
- Verify collection iteration

### 5. Integration Tests
- Test complete tracking workflow
- Test lifecycle event handling
- Verify persistence operations
- Test error handling

## Placeholder Replacement Rules

1. **{ObjectType}**: Replace with actual Windchill object type (e.g., WTChangeOrder2)
2. **{domain}**: Replace with object domain (e.g., change2)
3. **{ObjectType}Master**: Replace with master object type if applicable
4. **{attribute_name}**: Replace with actual object attribute names from object.json

## Mock Setup Patterns

### Basic Mock Setup:
```java
@Mock
private {ObjectType} mockObject;

@Mock
private {ObjectType}Master mockMaster;

@Mock
private BACTrackingRecord mockTrackingRecord;

@Before
public void setUp() throws Exception {
    MockitoAnnotations.initMocks(this);
    delegate = new BAC{ObjectType}TrackingDelegate();
    when(mockObject.getMaster()).thenReturn(mockMaster);
}
```

### Attribute Mock Setup:
```java
// For each attribute in object.json
when(mockObject.get{AttributeName}()).thenReturn(mockValue);
```

## Validation Steps

1. **Syntax Validation**: Ensure generated test compiles without errors
2. **Import Verification**: Confirm all required imports are present
3. **Package Declaration**: Verify correct package structure
4. **Test Coverage**: Ensure all delegate methods are tested
5. **Mock Verification**: Confirm proper mock setup and verification
6. **Exception Handling**: Test error scenarios and edge cases

## Key Testing Principles

1. **Test Isolation**: Each test should be independent
2. **Mock Usage**: Use mocks for external dependencies
3. **Assertion Clarity**: Use descriptive assertion messages
4. **Edge Cases**: Test null, empty, and invalid inputs
5. **Workflow Testing**: Test complete tracking workflows
6. **Error Handling**: Verify exception handling

## Notes

- All tests should follow JUnit 4 conventions
- Use Mockito for mocking dependencies
- Include JavaDoc comments for clarity
- Follow BAC naming conventions
- Ensure proper setup and teardown
- Test both positive and negative scenarios
- Verify tracking record creation and management
- Test lifecycle event handling
