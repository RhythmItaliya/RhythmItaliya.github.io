# RhythmItaliya.github.io
// MySlider.js
import React from 'react';
import { Slider, Table, Alert } from 'antd';
import { useDispatch, useSelector } from 'react-redux';
import { setPriceRange } from './redux/action';
import products from './Data';

const MySlider = () => {
  const dispatch = useDispatch();
  const rangeValue = useSelector((state) => state.slider.rangeValue);

  const filteredProducts = products.filter((product) => product.price >= rangeValue[0] && product.price <= rangeValue[1]);

  const columns = [
    {
      title: 'Product Name',
      dataIndex: 'productName',
      key: 'productName',
    },
    {
      title: 'Price',
      dataIndex: 'price',
      key: 'price',
      render: (price) => `$${price.toFixed(2)}`,
    },
    {
      title: 'Description',
      dataIndex: 'description',
      key: 'description',
    },
  ];

  const handleSliderChange = (value) => {
    dispatch(setPriceRange(value));
  };

  return (
    <div style={{ width: '50%', margin: 'auto' }}>
      <Slider min={1} max={100} range defaultValue={rangeValue} onChange={handleSliderChange} />
      {filteredProducts.length > 0 ? (
        <Table dataSource={filteredProducts} columns={columns} pagination={{ pageSize: 4 }} />
      ) : (
        <Alert message="No products available in the selected range." type="info" />
      )}
    </div>
  );
};

export default MySlider;
