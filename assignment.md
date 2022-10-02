# Frontend Engineer Assignment by V S Ravi Kiran

## **React Component Code Review**
#### **1. Explain what the simple` List `component does.**

>  Lists can be created in a similar way as we create lists in JavaScript.The list component contains list items.
> List is used to store multiple items of the same and different datatype under a variable like array .
> Here single list item is stored inside WrappedSingleListItem. React.memo(WrappedListComponent) returns a new memoized component List. List render is memoized but the outputs of List is same content as the original WrappedListComponent component.items array is used to store these list items and then each item is looped through using map() function and displayed. The background color of text changes according to the isSelected stat

e, which is dependent on selectedIndex..
#### **2. What problems / warnings are there with code?**

1. Defining a default prop as null is not recommended. Either use Undefined or an array of values.

   `WrappedListComponent.defaultProps = {
  items: null,
    };`
 
2. In list item's onClick method there is a function call but onClick accepts function's reference.
    ` <li style={{ backgroundColor: isSelected ? "green" : "red" }}
      onClick={onClickHandler(index)}>
       {text}
       </li>`
3. Syntax error in useState() hook
     ` const [setSelectedIndex, selectedIndex] = useState(); `

#### **3. Please fix, optimize, and/or modify the component as much as you think is necessary.**
```
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "green" : "red" }}
      onClick={onClickHandler}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: "left" }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          // index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};

WrappedListComponent.defaultProps = {
  items: [{ text: "Shivani" }, { text: "sai krishna" }, { text: "Tharun" }],
};

const List = memo(WrappedListComponent);

export default List;
```
