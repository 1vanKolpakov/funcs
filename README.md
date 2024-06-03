# funcs

const transformMatrixToObject = (matrix) => {
  const uniqueItems = matrix.reduce((accumulator, current) => {
    Object.entries(current).forEach(([key, value]) => {
      if (key === 'UF_PRODUCT' && value.UF_NAME === 'Ничего из вышеперечисленного') {
        if (!accumulator['UF_PRODUCT_NIV']) {
          accumulator['UF_PRODUCT_NIV'] = { UF_NAME: 'Ничего из вышеперечисленного', values: new Set() };
        }
        accumulator['UF_PRODUCT_NIV'].values.add(value.UF_CODE);
      } else {
        if (!accumulator[key]) {
          accumulator[key] = new Map();
        }
        if (!accumulator[key].has(value.ID)) {
          accumulator[key].set(value.ID, value);
        }
      }
    });
    return accumulator;
  }, {});

  const result = {};
  Object.entries(uniqueItems).forEach(([key, valueMap]) => {
    if (key === 'UF_PRODUCT_NIV') {
      result['UF_PRODUCT'] = [{ UF_NAME: 'Ничего из вышеперечисленного', UF_CODES: Array.from(valueMap.values) }];
    } else {
      result[key] = Array.from(valueMap.values());
    }
  });

  return result;
};
