# Wilderness-calculator
Wilderness-calculator
import React, { useState } from 'react';
import { Camera } from 'lucide-react';

const WildernessCalculator = () => {
  const [display, setDisplay] = useState('0');
  const [prevValue, setPrevValue] = useState(null);
  const [operator, setOperator] = useState(null);
  const [waitingForOperand, setWaitingForOperand] = useState(false);

  const calculate = (a, b, op) => {
    switch (op) {
      case '+': return a + b;
      case '-': return a - b;
      case '×': return a * b;
      case '÷': return b !== 0 ? a / b : 'Error';
      default: return b;
    }
  };

  const handleNumber = (num) => {
    if (waitingForOperand) {
      setDisplay(String(num));
      setWaitingForOperand(false);
    } else {
      setDisplay(display === '0' ? String(num) : display + num);
    }
  };

  const handleOperator = (op) => {
    const inputValue = parseFloat(display);

    if (prevValue === null) {
      setPrevValue(inputValue);
    } else if (operator) {
      const result = calculate(prevValue, inputValue, operator);
      setPrevValue(result);
      setDisplay(String(result));
    }

    setWaitingForOperand(true);
    setOperator(op);
  };

  const handleEqual = () => {
    const inputValue = parseFloat(display);
    if (prevValue !== null && operator) {
      const result = calculate(prevValue, inputValue, operator);
      setDisplay(String(result));
      setPrevValue(null);
      setOperator(null);
      setWaitingForOperand(true);
    }
  };

  const clearDisplay = () => {
    setDisplay('0');
    setPrevValue(null);
    setOperator(null);
    setWaitingForOperand(false);
  };

  return (
    <div className="w-72 bg-gradient-to-b from-green-800 to-green-900 p-4 rounded-lg shadow-xl">
      <div className="flex justify-between items-center mb-4">
        <Camera className="text-green-300" size={24} />
        <span className="text-green-300 font-semibold">Wildlife Calculator</span>
      </div>
      
      <div className="bg-green-100 p-3 rounded-lg mb-4 text-right text-2xl font-bold text-green-900 h-14 flex items-center justify-end">
        {display}
      </div>

      <div className="grid grid-cols-4 gap-2">
        <button onClick={clearDisplay} className="col-span-2 bg-red-600 text-white p-2 rounded-lg hover:bg-red-700 flex items-center justify-center">
          Clear
        </button>
        <button onClick={() => handleOperator('÷')} className="bg-green-700 text-white p-2 rounded-lg hover:bg-green-800 font-bold">
          ÷
        </button>
        <button onClick={() => handleOperator('×')} className="bg-green-700 text-white p-2 rounded-lg hover:bg-green-800 font-bold">
          ×
        </button>

        {[7, 8, 9].map(num => (
          <button
            key={num}
            onClick={() => handleNumber(num)}
            className="bg-green-600 text-white p-2 rounded-lg hover:bg-green-700"
          >
            {num}
          </button>
        ))}
        <button onClick={() => handleOperator('-')} className="bg-green-700 text-white p-2 rounded-lg hover:bg-green-800 font-bold">
          -
        </button>

        {[4, 5, 6].map(num => (
          <button
            key={num}
            onClick={() => handleNumber(num)}
            className="bg-green-600 text-white p-2 rounded-lg hover:bg-green-700"
          >
            {num}
          </button>
        ))}
        <button onClick={() => handleOperator('+')} className="bg-green-700 text-white p-2 rounded-lg hover:bg-green-800 font-bold">
          +
        </button>

        {[1, 2, 3].map(num => (
          <button
            key={num}
            onClick={() => handleNumber(num)}
            className="bg-green-600 text-white p-2 rounded-lg hover:bg-green-700"
          >
            {num}
          </button>
        ))}
        <button
          onClick={handleEqual}
          className="bg-yellow-600 text-white p-2 rounded-lg hover:bg-yellow-700 row-span-2 font-bold"
        >
          =
        </button>

        <button
          onClick={() => handleNumber(0)}
          className="col-span-2 bg-green-600 text-white p-2 rounded-lg hover:bg-green-700"
        >
          0
        </button>
        <button
          onClick={() => handleNumber('.')}
          className="bg-green-600 text-white p-2 rounded-lg hover:bg-green-700 font-bold"
        >
          .
        </button>
      </div>
    </div>
  );
};

export default WildernessCalculator;
