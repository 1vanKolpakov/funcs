# funcs

const transformMatrixToObject = (matrix) => {
  const uniqueItems = matrix.reduce((accumulator, current) => {
    Object.entries(current).forEach(([key, value]) => {
      // Check for "Ничего из вышеперечисленного" in UF_PRODUCT or UF_NAME
      if (key === 'UF_PRODUCT' && value.name === 'Ничего из вышеперечисленного') {
        if (!accumulator['OTHER_UF_PRODUCT']) {
          accumulator['OTHER_UF_PRODUCT'] = new Map();
        }
        if (!accumulator['OTHER_UF_PRODUCT'].has(value.id)) {
          accumulator['OTHER_UF_PRODUCT'].set(value.id, value);
        }
      } else if (key === 'UF_NAME' && value.name === 'Ничего из вышеперечисленного') {
        if (!accumulator['OTHER_UF_NAME']) {
          accumulator['OTHER_UF_NAME'] = new Map();
        }
        if (!accumulator['OTHER_UF_NAME'].has(value.id)) {
          accumulator['OTHER_UF_NAME'].set(value.id, value);
        }
      } else {
        if (!accumulator[key]) {
          accumulator[key] = new Map();
        }
        if (!accumulator[key].has(value.id)) {
          accumulator[key].set(value.id, value);
        }
      }
    });
    return accumulator;
  }, {});

  const result = {};
  Object.entries(uniqueItems).forEach(([key, valueMap]) => {
    result[key] = Array.from(valueMap.values());
  });

  return result;
};
