const processArray = (arr) => {
  const nameCount = arr.reduce((acc, item) => {
    acc[item.UF_NAME] = (acc[item.UF_NAME] || 0) + 1;
    return acc;
  }, {});

  const seen = new Set();
  return arr.reduce((acc, item) => {
    if (nameCount[item.UF_NAME] > 1) {
      if (!seen.has(item.UF_NAME)) {
        seen.add(item.UF_NAME);
        acc.push(item);
      }
    } else {
      acc.push(item);
    }
    return acc;
  }, []).map(item => {
    if (nameCount[item.UF_NAME] > 1 && seen.has(item.UF_NAME)) {
      const { UF_NAME, UF_CODE, ...rest } = item;
      return rest;
    }
    return item;
  });
};
