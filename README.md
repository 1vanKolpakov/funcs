# funcs
const filterMatrix = (matrix, filters) => {
  return matrix.filter(item => {
    return Object.entries(filters).every(([key, filterValues]) => {
      const itemValue = item[key];
      if (!itemValue) return false;

      return filterValues.some(filterValue => {
        return Object.entries(filterValue).every(([filterKey, filterVal]) => {
          return itemValue[filterKey] === filterVal;
        });
      });
    });
  });
};

  return result;
};
