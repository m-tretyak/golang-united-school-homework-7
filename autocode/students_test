package coverage

import (
	"os"
	"reflect"
	"testing"
	"time"
)

// DO NOT EDIT THIS FUNCTION
func init() {
	content, err := os.ReadFile("students_test.go")
	if err != nil {
		panic(err)
	}
	err = os.WriteFile("autocode/students_test", content, 0644)
	if err != nil {
		panic(err)
	}
}

// WRITE YOUR CODE BELOW

func testPanic(testFunc func()) (errMsg interface{}, isPanic bool) {
	defer func() {
		if err := recover(); err != nil {
			errMsg = err
			isPanic = true
		}
	}()
	testFunc()
	return nil, false
}

func TestPeople_Len(t *testing.T) {
	cases := map[string]struct {
		People
		expected int
	}{
		"len for nil people": {
			expected: 0,
			People:   nil,
		},
		"len for empty people": {
			expected: 0,
			People:   []Person{},
		},
		"len for single person's people": {
			expected: 1,
			People:   []Person{{"", "", time.Time{}}},
		},
		"len for three person's people": {
			expected: 3,
			People: []Person{
				{"", "", time.Time{}},
				{"", "", time.Time{}},
				{"", "", time.Time{}},
			},
		},
	}

	for name, tc := range cases {
		data := tc
		t.Run(name, func(tt *testing.T) {
			actualValue := data.Len()
			if actualValue == data.expected {
				return
			}

			tt.Errorf("'%s' test case, expected %v but got %v ", name, data.expected, actualValue)
		})
	}
}

func TestPeople_Swap(t *testing.T) {
	cases := map[string]struct {
		People
		i, j    int
		isPanic bool
	}{
		"swap for nil people": {
			isPanic: true,
			People:  nil,
		},
		"swap for empty people": {
			isPanic: true,
			People:  []Person{},
		},
		"swap for single person's people": {
			isPanic: false, i: 0, j: 0,
			People: []Person{{}},
		},
		"swap 0 <=> 1 for two person's people": {
			isPanic: false, i: 0, j: 1, People: []Person{
				{"fn0", "ln0", time.Time{}.AddDate(0, 0, 0)},
				{"fn1", "ln1", time.Time{}.AddDate(1, 1, 1)},
			},
		},
		"swap 0 <=> 2, panic for two person's people": {
			isPanic: true, i: 0, j: 2, People: []Person{
				{"fn0", "ln0", time.Time{}.AddDate(0, 0, 0)},
				{"fn1", "ln1", time.Time{}.AddDate(1, 1, 1)},
			},
		},
		"swap 0 <=> 2, for three person's people": {
			isPanic: false, i: 0, j: 2, People: []Person{
				{"fn0", "ln0", time.Time{}.AddDate(0, 0, 0)},
				{"fn1", "ln1", time.Time{}.AddDate(1, 1, 1)},
				{"fn2", "ln2", time.Time{}.AddDate(2, 2, 2)},
			},
		},
	}

	for name, tc := range cases {
		data := tc
		t.Run(name, func(tt *testing.T) {
			var oldI, oldJ, actI, actJ Person

			errMsg, isPanic := testPanic(func() {
				oldI = data.People[data.i]
				oldJ = data.People[data.j]
				data.People.Swap(data.i, data.j)
			})

			if data.isPanic != isPanic {
				tt.Errorf("'%s' test case, panic handler, panic expected '%t', occurs '%t' %s", name, data.isPanic, isPanic, errMsg)
			}

			if isPanic {
				return
			}

			actI = data.People[data.i]
			actJ = data.People[data.j]

			if !reflect.DeepEqual(oldI, actJ) {
				tt.Errorf("'%s' test case, no swap (%d,%d) occurs %+v with %+v", name, data.i, data.j, oldI, actJ)
			}

			if !reflect.DeepEqual(oldJ, actI) {
				tt.Errorf("'%s' test case, no swap (%d,%d) occurs %+v with %+v", name, data.j, data.i, oldJ, actI)
			}
		})
	}
}

func TestPeople_Less(t *testing.T) {
	unix0 := time.Unix(0, 0)
	cases := map[string]struct {
		People
		i, j     int
		expected bool
		isPanic  bool
	}{
		"less for nil people": {
			isPanic: true,
			People:  nil,
		},
		"less for empty people": {
			isPanic: true,
			People:  []Person{},
		},
		"less for single person's people": {
			isPanic: false, i: 0, j: 0, expected: false,
			People: []Person{{}},
		},
		"less for self person": {
			isPanic: false, i: 1, j: 1, expected: false,
			People: []Person{
				{"fn0", "ln0", unix0.AddDate(1, 1, 1)},
				{"fn1", "ln1", unix0.AddDate(0, 0, 0)},
			},
		},
		"less for (0,0,1) << (1,1,0)": {
			isPanic: false, i: 0, j: 1, expected: true,
			People: []Person{
				{"fn0", "ln0", unix0.AddDate(1, 1, 1)},
				{"fn1", "ln1", unix0.AddDate(0, 0, 0)},
			},
		},
		"less for (0,0,1) << (1,1,1)": {
			isPanic: false, i: 0, j: 1, expected: true,
			People: []Person{
				{"fn0", "ln0", unix0.AddDate(1, 1, 1)},
				{"fn1", "ln1", unix0.AddDate(1, 1, 1)},
			},
		},
		"less for (1,0,1) << (1,1,1)": {
			isPanic: false, i: 0, j: 1, expected: true,
			People: []Person{
				{"fn1", "ln0", unix0.AddDate(1, 1, 1)},
				{"fn1", "ln1", unix0.AddDate(1, 1, 1)},
			},
		},
		"less for (1,1,1) << (1,0,1)": {
			isPanic: false, i: 0, j: 1, expected: false,
			People: []Person{
				{"fn1", "ln1", unix0.AddDate(1, 1, 1)},
				{"fn1", "ln0", unix0.AddDate(1, 1, 1)},
			},
		},
	}

	for name, tc := range cases {
		data := tc
		t.Run(name, func(tt *testing.T) {
			actual := false

			errMsg, isPanic := testPanic(func() {
				actual = data.People.Less(data.i, data.j)
			})

			if data.isPanic != isPanic {
				tt.Errorf("'%s' test case, panic handler, panic expected '%t', occurs '%t' %s",
					name, data.isPanic, isPanic, errMsg)
			}

			if isPanic {
				return
			}

			if actual != data.expected {
				tt.Errorf("'%s' test case, expected '%t', got '%t' for: \n%+v\n%+v",
					name, data.expected, actual, data.People[data.i], data.People[data.j])
			}
		})
	}
}

/////////////////////////////////////////////////////////////////////////////////////////////////

func TestMatrix_New(t *testing.T) {
	cases := map[string]struct {
		input         string
		expected      *Matrix
		expectedError bool
		expectedPanic bool
	}{
		"incorrect input, empty": {
			input:         "",
			expectedError: true},
		"incorrect input, missed last row element": {
			input:         "1 2\n3",
			expectedError: true},
		"incorrect input, missed first row element": {
			input:         "1\n2 3",
			expectedError: true},
		"incorrect input, mistype element in a row": {
			input:         "1 2\n3 z4",
			expectedError: true},
		"correct matrix 3x2": {
			input:    "1 2\n3 4\n5 6",
			expected: &Matrix{3, 2, []int{1, 2, 3, 4, 5, 6}}},
		"correct matrix 2x2": {
			input:    "1 2\n3 4",
			expected: &Matrix{2, 2, []int{1, 2, 3, 4}}},
		"correct matrix 1x1": {
			input:    "42",
			expected: &Matrix{1, 1, []int{42}}},
		"correct matrix, single column": {
			input:    "1\n2\n3\n4",
			expected: &Matrix{4, 1, []int{1, 2, 3, 4}}},
		"correct matrix, single row": {
			input:    "1 2 3 4",
			expected: &Matrix{1, 4, []int{1, 2, 3, 4}}},
	}

	for name, tc := range cases {
		data := tc
		t.Run(name, func(tt *testing.T) {
			m, err := New(data.input)
			if data.expectedError {
				if err == nil {
					tt.Fatal("error expected")
				}
				if m != nil {
					tt.Fatal("error expected, non-nil result unexpected")
				}

				return
			}

			if err != nil {
				tt.Fatal("nil error expected, got error ", err)
			}

			if m == nil {
				tt.Fatal("nil result is unexpected")
			}

			if !reflect.DeepEqual(*m, *data.expected) {
				tt.Fatalf("unexpected result of New(), expected: %v got: %v", *data.expected, *m)
			}
		})
	}
}

func TestMatrix_Rows(t *testing.T) {
	cases := map[string]struct {
		input    string
		expected *[][]int
	}{
		"matrix 2x2": {
			"1 2\n3 4",
			&[][]int{{1, 2}, {3, 4}},
		},
		"matrix 1x1": {
			"42",
			&[][]int{{42}},
		},
		"single column 4x1": {
			"1\n2\n3\n4",
			&[][]int{{1}, {2}, {3}, {4}},
		},
		"single row 1x4": {
			"1 2 3 4",
			&[][]int{{1, 2, 3, 4}},
		},
	}

	for name, tc := range cases {
		data := tc
		t.Run(name, func(tt *testing.T) {
			m, err := New(data.input)

			if err != nil {
				tt.Fatal("unexpected error ", err)
			}

			if m == nil {
				tt.Fatal("nil result is unexpected")
			}

			rows := m.Rows()

			if rows == nil {
				tt.Fatal("nil result is unexpected for Rows()")
			}

			if !reflect.DeepEqual(rows, *data.expected) {
				tt.Fatalf("unexpected result of Rows(), expected: %v got: %v", *data.expected, rows)
			}
		})
	}
}

func TestMatrix_Cols(t *testing.T) {
	cases := map[string]struct {
		input    string
		expected *[][]int
	}{
		"matrix 2x2": {
			"1 2\n3 4",
			&[][]int{{1, 3}, {2, 4}},
		},
		"matrix 1x1": {
			`42`,
			&[][]int{{42}},
		},
		"single column 4x1": {
			"1\n2\n3\n4",
			&[][]int{{1, 2, 3, 4}},
		},
		"single row 1x4": {
			"1 2 3 4",
			&[][]int{{1}, {2}, {3}, {4}},
		},
	}

	for name, tc := range cases {
		data := tc
		t.Run(name, func(t *testing.T) {
			m, err := New(data.input)

			if err != nil {
				t.Fatal("unexpected error ", err)
			}

			if m == nil {
				t.Fatal("nil result is unexpected")
			}

			cols := m.Cols()

			if cols == nil {
				t.Fatal("nil result is unexpected for Cols()")
			}

			if !reflect.DeepEqual(cols, *data.expected) {
				t.Fatalf("unexpected result for Cols() expected: %v got: %v", *data.expected, cols)
			}
		})
	}
}

func TestMatrix_Set(t *testing.T) {
	cases := map[string]struct {
		row, col, value int
		matrix          *Matrix
		expectedMatrix  *Matrix
		expectedResult  bool
	}{
		"incorrect target index (-1, 0)": {
			-1, 0, 42,
			&Matrix{2, 2, []int{1, 2, 3, 4}},
			&Matrix{2, 2, []int{1, 2, 3, 4}},
			false,
		},
		"incorrect target index (2, 0)": {
			2, 0, 42,
			&Matrix{2, 2, []int{1, 2, 3, 4}},
			&Matrix{2, 2, []int{1, 2, 3, 4}},
			false,
		},
		"incorrect target index (0, -1)": {
			0, -1, 42,
			&Matrix{2, 2, []int{1, 2, 3, 4}},
			&Matrix{2, 2, []int{1, 2, 3, 4}},
			false,
		},
		"incorrect target index (0, 2)": {
			0, 2, 42,
			&Matrix{2, 2, []int{1, 2, 3, 4}},
			&Matrix{2, 2, []int{1, 2, 3, 4}},
			false,
		},
		"correct target index (0, 0)": {
			0, 0, 42,
			&Matrix{2, 2, []int{1, 2, 3, 4}},
			&Matrix{2, 2, []int{42, 2, 3, 4}},
			true,
		},
		"correct target index (0, 1)": {
			0, 1, 42,
			&Matrix{2, 2, []int{1, 2, 3, 4}},
			&Matrix{2, 2, []int{1, 42, 3, 4}},
			true,
		},
		"correct target index (1, 0)": {
			1, 0, 42,
			&Matrix{2, 2, []int{1, 2, 3, 4}},
			&Matrix{2, 2, []int{1, 2, 42, 4}},
			true,
		},
		"correct target index (1, 1)": {
			1, 1, 42,
			&Matrix{2, 2, []int{1, 2, 3, 4}},
			&Matrix{2, 2, []int{1, 2, 3, 42}},
			true,
		},
	}

	for name, tc := range cases {
		data := tc
		t.Run(name, func(tt *testing.T) {
			result := data.matrix.Set(data.row, data.col, data.value)

			if result != data.expectedResult {
				tt.Fatalf("unexpected result, expected: %v, got: %v", data.expectedResult, result)
			}

			if !reflect.DeepEqual(*data.expectedMatrix, *data.matrix) {
				tt.Fatalf("unexpected matrix results, expected: %v got: %v", *data.expectedMatrix, *data.matrix)
			}
		})
	}
}
