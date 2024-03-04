#include <iostream>
#include <string>
#include <cctype>

using namespace std;

class Parser {
public:
    Parser(const string& input) : input(input), pos(0), result(false) {}

    bool parse() {
        result = parseExpr() && pos == input.size();
        return result;
    }

    bool getResult() const {
        return result;
    }

    string getProcess() const {
        return process;
    }

    string getInput() const {
        return input;
    }

private:
    string input;
    size_t pos;
    bool result;
    string process;

    bool parseExpr() {
        process += "parseExpr()\n";
        return parseTerm() && parseExprTail();
    }

    bool parseExprTail() {
        process += "parseExprTail()\n";
        if (pos >= input.size()) return true;

        char op = input[pos];
        if (op == '+' || op == '-') {
            pos++;
            return parseTerm() && parseExprTail();
        }
        return true;
    }

    bool parseTerm() {
        process += "parseTerm()\n";
        return parseFactor() && parseTermTail();
    }

    bool parseTermTail() {
        process += "parseTermTail()\n";
        if (pos >= input.size()) return true;

        char op = input[pos];
        if (op == '*' || op == '/') {
            pos++;
            return parseFactor() && parseTermTail();
        }
        return true;
    }

    bool parseFactor() {
        process += "parseFactor()\n";
        if (pos >= input.size()) return false;

        if (isdigit(input[pos])) {
            while (pos < input.size() && isdigit(input[pos])) {
                pos++;
            }
            return true;
        } else if (input[pos] == '(') {
            pos++;
            bool result = parseExpr();
            if (pos < input.size() && input[pos] == ')') {
                pos++;
                return result;
            }
        }
        return false;
    }
};

int main() {
    string input1, input2;

    cout << "Enter input1: ";
    getline(cin, input1);

    cout << "Enter input2: ";
    getline(cin, input2);

    Parser parser1(input1);
    Parser parser2(input2);

    bool result1 = parser1.parse();
    bool result2 = parser2.parse();

    cout << "Parsing input1: " << (result1 ? "Success" : "Failure") << endl;
    cout << "Parsing input2: " << (result2 ? "Success" : "Failure") << endl;

    cout << "Parser result for input1: " << parser1.getResult() << endl;
    cout << "Parser result for input2: " << parser2.getResult() << endl;

    cout << "Parsing process for input1:\n" << parser1.getProcess() << endl;
    cout << "Parsing process for input2:\n" << parser2.getProcess() << endl;

    cout << "Parsing input1: " << parser1.getInput() << endl;
    cout << "Parsing input2: " << parser2.getInput() << endl;

    return 0;
}
