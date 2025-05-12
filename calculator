import 'package:flutter/material.dart';
import 'package:share_plus/share_plus.dart';
import 'dart:math';

void main() {
  runApp(HAICalc());
}

class HAICalc extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: "H AI Calc",
      theme: ThemeData(
        primarySwatch: Colors.teal,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: WelcomeScreen(),
    );
  }
}

class WelcomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              "H AI Calc",
              style: TextStyle(fontSize: 32, fontWeight: FontWeight.bold, color: Colors.teal),
            ),
            SizedBox(height: 20),
            Text(
              "Smart Calculations by Zakir Hussain Akhoon",
              style: TextStyle(fontSize: 18),
              textAlign: TextAlign.center,
            ),
            SizedBox(height: 40),
            ElevatedButton(
              onPressed: () {
                Navigator.pushReplacement(
                  context,
                  MaterialPageRoute(builder: (context) => CalculatorHome()),
                );
              },
              child: Text("Start Calculating"),
              style: ElevatedButton.styleFrom(
                backgroundColor: Colors.teal,
                padding: EdgeInsets.symmetric(horizontal: 20, vertical: 15),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class CalculatorHome extends StatefulWidget {
  @override
  _CalculatorHomeState createState() => _CalculatorHomeState();
}

class _CalculatorHomeState extends State<CalculatorHome> {
  String _output = "0";
  String _currentNumber = "";
  String _operation = "";
  double _num1 = 0;
  double _num2 = 0;
  String _mode = "Basic"; // Basic, Scientific, Conversion, AI
  String _conversionType = "USD to INR";
  Map<String, double> _conversionRates = {
    "USD to INR": 83.5,
    "Meter to Feet": 3.28084,
  };
  List<String> _history = [];

  void buttonPressed(String buttonText) {
    try {
      if (_mode == "Basic" || _mode == "Scientific") {
        if (buttonText == "CLEAR") {
          _output = "0";
          _currentNumber = "";
          _operation = "";
          _num1 = 0;
          _num2 = 0;
        } else if (buttonText == "+" || buttonText == "-" || buttonText == "×" || buttonText == "÷") {
          _num1 = double.parse(_output);
          _operation = buttonText;
          _currentNumber = "";
        } else if (buttonText == "=") {
          _num2 = double.parse(_output);
          String result;
          if (_operation == "+") {
            result = (_num1 + _num2).toStringAsFixed(2);
          } else if (_operation == "-") {
            result = (_num1 - _num2).toStringAsFixed(2);
          } else if (_operation == "×") {
            result = (_num1 * _num2).toStringAsFixed(2);
          } else if (_operation == "÷") {
            if (_num2 == 0) throw Exception("Cannot divide by zero");
            result = (_num1 / _num2).toStringAsFixed(2);
          } else {
            result = _output;
          }
          _history.add("$_num1 $_operation $_num2 = $result");
          _output = result;
          _operation = "";
          _currentNumber = _output;
        } else if (buttonText == "sin" || buttonText == "cos" || buttonText == "log") {
          double value = double.parse(_output);
          String result;
          if (buttonText == "sin") {
            result = sin(value * pi / 180).toStringAsFixed(4);
          } else if (buttonText == "cos") {
            result = cos(value * pi / 180).toStringAsFixed(4);
          } else if (buttonText == "log") {
            if (value <= 0) throw Exception("Invalid input for log");
            result = (log(value) / log(10)).toStringAsFixed(4);
          } else {
            result = _output;
          }
          _history.add("$buttonText($value) = $result");
          _output = result;
          _currentNumber = _output;
        } else {
          _currentNumber += buttonText;
          _output = _currentNumber;
        }
      } else if (_mode == "Conversion") {
        _currentNumber += buttonText;
        double value = double.parse(_currentNumber.isEmpty ? "0" : _currentNumber);
        double rate = _conversionRates[_conversionType] ?? 1.0;
        String result = (value * rate).toStringAsFixed(2);
        _history.add("$value $_conversionType = $result");
        _output = result;
      }
    } catch (e) {
      _output = "Error: ${e.toString().split(':').last.trim()}";
      _history.add(_output);
    }
    setState(() {});
  }

  void switchMode(String mode) {
    setState(() {
      _mode = mode;
      _output = "0";
      _currentNumber = "";
      _operation = "";
    });
  }

  void changeConversionType(String type) {
    setState(() {
      _conversionType = type;
      _currentNumber = "";
      _output = "0";
    });
  }

  void shareResult() {
    String shareText = _mode == "Basic" || _mode == "Scientific"
        ? "Just calculated $_output with H AI Calc! 🚀 Try it now!"
        : "Converted $_currentNumber $_conversionType to $_output with H AI Calc! 🌟";
    Share.share("$shareText Download at bit.ly/HAICalc #AICalculator #AndroidApp");
  }

  void simulateAIQuery(String type) {
    String result;
    if (type == "Budget") {
      String mockQuery = "Save 10,000 monthly for 3 years at 6% interest";
      double monthlySavings = 10000;
      int months = 36;
      double rate = 0.06;
      double futureValue = monthlySavings * ((pow(1 + rate / 12, months) - 1) / (rate / 12));
      result = futureValue.toStringAsFixed(2);
      _history.add("AI: $mockQuery = ₹$result");
    } else {
      String mockQuery = "EMI for 5 lakh loan at 7% for 3 years";
      double principal = 500000;
      double rate = 0.07 / 12;
      int months = 36;
      double emi = principal * rate * pow(1 + rate, months) / (pow(1 + rate, months) - 1);
      result = emi.toStringAsFixed(2);
      _history.add("AI: $mockQuery = ₹$result");
    }
    setState(() {
      _output = result;
    });
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(content: Text("AI Result: $result (Premium Feature)")),
    );
  }

  void showHistory() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: Text("Calculation History"),
        content: Container(
          width: double.maxFinite,
          height: 300,
          child: ListView.builder(
            itemCount: _history.length,
            itemBuilder: (context, index) => ListTile(
              title: Text(_history[index]),
            ),
          ),
        ),
        actions: [
          TextButton(
            onPressed: () {
              setState(() {
                _history.clear();
              });
              Navigator.pop(context);
            },
            child: Text("Clear History"),
          ),
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: Text("Close"),
          ),
        ],
      ),
    );
  }

  Widget buildButton(String buttonText, {double fontSize = 20}) {
    return Expanded(
      child: Padding(
        padding: EdgeInsets.all(2),
        child: OutlinedButton(
          child: Text(buttonText, style: TextStyle(fontSize: fontSize)),
          onPressed: () => buttonPressed(buttonText),
          style: OutlinedButton.styleFrom(
            side: BorderSide(color: Colors.teal),
          ),
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("H AI Calc by Zakir Hussain Akhoon"),
        centerTitle: true,
        actions: [
          IconButton(
            icon: Icon(Icons.history),
            onPressed: showHistory,
          ),
        ],
      ),
      body: Column(
        children: [
          Container(
            alignment: Alignment.centerRight,
            padding: EdgeInsets.symmetric(vertical: 24, horizontal: 12),
            child: Text(
              _output,
              style: TextStyle(fontSize: 48, fontWeight: FontWeight.bold),
            ),
          ),
          Expanded(child: Divider()),
          Row(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              ElevatedButton(
                onPressed: () => switchMode("Basic"),
                child: Text("Basic"),
                style: ElevatedButton.styleFrom(backgroundColor: Colors.teal),
              ),
              SizedBox(width: 10),
              ElevatedButton(
                onPressed: () => switchMode("Scientific"),
                child: Text("Scientific"),
                style: ElevatedButton.styleFrom(backgroundColor: Colors.teal),
              ),
              SizedBox(width: 10),
              ElevatedButton(
                onPressed: () => switchMode("Conversion"),
                child: Text("Conversion"),
                style: ElevatedButton.styleFrom(backgroundColor: Colors.teal),
              ),
            ],
          ),
          Row(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              ElevatedButton(
                onPressed: () => simulateAIQuery("Budget"),
                child: Text("AI Budget"),
                style: ElevatedButton.styleFrom(backgroundColor: Colors.tealAccent),
              ),
              SizedBox(width: 10),
              ElevatedButton(
                onPressed: () => simulateAIQuery("EMI"),
                child: Text("AI EMI"),
                style: ElevatedButton.styleFrom(backgroundColor: Colors.tealAccent),
              ),
            ],
          ),
          if (_mode == "Conversion")
            Padding(
              padding: EdgeInsets.all(10),
              child: DropdownButton<String>(
                value: _conversionType,
                items: _conversionRates.keys.map((String type) {
                  return DropdownMenuItem<String>(
                    value: type,
                    child: Text(type),
                  );
                }).toList(),
                onChanged: (value) => changeConversionType(value!),
              ),
            ),
          Expanded(child: Divider()),
          Column(
            children: [
              Row(children: [
                buildButton("7"),
                buildButton("8"),
                buildButton("9"),
                buildButton("÷"),
              ]),
              Row(children: [
                buildButton("4"),
                buildButton("5"),
                buildButton("6"),
                buildButton("×"),
              ]),
              Row(children: [
                buildButton("1"),
                buildButton("2"),
                buildButton("3"),
                buildButton("-"),
              ]),
              Row(children: [
                buildButton("0"),
                buildButton("."),
                buildButton("="),
                buildButton("+"),
              ]),
              if (_mode == "Scientific")
                Row(children: [
                  buildButton("sin", fontSize: 16),
                  buildButton("cos", fontSize: 16),
                  buildButton("log", fontSize: 16),
                  buildButton("π", fontSize: 16),
                ]),
              Row(children: [
                buildButton("CLEAR"),
                Expanded(
                  child: Padding(
                    padding: EdgeInsets.all(2),
                    child: ElevatedButton(
                      child: Text("SHARE", style: TextStyle(fontSize: 20)),
                      onPressed: shareResult,
                      style: ElevatedButton.styleFrom(backgroundColor: Colors.teal),
                    ),
                  ),
                ),
              ]),
            ],
          ),
        ],
      ),
    );
  }
}
