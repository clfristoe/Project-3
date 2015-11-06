	#include <iostream>
	#include <fstream>
	#include <iomanip>
	using namespace std;

	void ReadInput(ifstream &, int [], float [], int&);
	void Print(ofstream &, int [], float [], int&);
	void BubbleSort(int [], float [], int &);
	void ExchangeSort(int [], float [], int &);
	void FindLowScore(float [], int &, int &);
	void FindHighScore(float [], int &, int &);
	void CalculateSum(float [], int &, float &);
	void PrintScores(ofstream &,float [], int [], int &, int &, float &, int &);
	void Footer(ofstream &);
	void Header(ofstream &);

	int main()
	{
	ifstream input ("DATA3.TXT", ios::in);
	ofstream output ("OUTPUT", ios::out);
	output.setf(ios::fixed);
	output.precision(1);
	int studentIDs[50]; //creates an array with fifty integer places for student id's
	float testScores[50]; //creates an array with fifty float places for test scores
	int EU;
	float sum;
	int highScore = 0; //Initialize highScore to zero
	int lowScore = 0; //Initialize lowScore to zero

	Header(output);
	ReadInput(input, studentIDs, testScores, EU);

	
	//Print out the original list of IDs and Scores
	output << "The original list of test scores is: "<< endl;
	output << endl;
	output << setw(6) << "Student ID #" << setw(12) <<"Test Score" << endl;
	Print(output, studentIDs, testScores, EU);
	output << endl;

	//Sort from High to low based on test scores
	BubbleSort(studentIDs, testScores, EU);

	//Print out the updated list of IDs and Scores
	output << "The test scores sorted highest to lowest are: "<< endl;
	output << endl;
	output << setw(6) << "Student ID #" << setw(12) <<"Test Score" << endl;
	Print(output, studentIDs, testScores, EU);
	output << endl;
	
	
	//Sort from low to high based on IDs
	ExchangeSort(studentIDs, testScores, EU);

	//Print out the updated list of IDs and Scores
	output << "The student ID numbers sorted lowest to highest are: "<< endl;
	output << endl;
	output << setw(6) << "Student ID #" << setw(12) <<"Test Score" << endl;
	Print(output, studentIDs, testScores, EU);
	output << endl;
	
	//Find the lowest test score and corrresponding ID
	FindLowScore(testScores, EU, lowScore);

	//Find the highest test score and corresponding ID
	FindHighScore(testScores, EU, highScore);

	//Find the average test score
	CalculateSum(testScores, EU, sum);

	//Print out low score, high score, and average test score

	PrintScores(output, testScores, studentIDs, lowScore, highScore, sum, EU);
	Footer(output);
	}

	void ReadInput(ifstream &infile, int IDs[], float scores[], int &elementsUsed)
			// Recieveds - the inputfile, array of student IDs, array of test scores, and elements used
			// Task - reads in the IDs and test scores from a file
			// Returns - filled arrays
	{
	int num; 
	float score;
	elementsUsed = 0;
	infile>>num; //read in the first student ID
	infile>>score; //read in the first test score
	while(num > 0) //Stay in loop while the placeholder value is greater than zero
	{
		if(num < 0 || score < 0) //check to see that both inputs are greater than zero
			return;
		IDs[elementsUsed] = num; //place the first ID into the array of student IDs
		scores[elementsUsed] = score; //place the first test score into the array of test scores
		infile >>num; //read in the next ID
		infile >>score; //read in the next test score
		elementsUsed++;
	}
	return;
	}

	void Print(ofstream &outfile, int IDs[], float scores[], int &elementsUsed)
			// Recieves - the output file, array of student IDs, array of test scores, and elements used
			// Task - Prints the array of student IDs and test scores in columns
			// Returns - Nothing
	{
	int i;
	for(i=0; i<elementsUsed; ++i) //continue in this loop until there are no more values
	{
		// print the student id's and test scores on the same line with equal spacing
		outfile << setw(5) << IDs[i]<<" "; 
		outfile << setw(15)<< scores[i];
		outfile << endl;
	}
	return;
	}
	
	void BubbleSort(int IDs[], float scores[], int &EU)     // Sorts list high to low
	{
    int M, N;
	float temp, temp2;
    for ( M = 0 ; M < EU-1; M++) // Loop through every element in the array
     {
		 for ( N = EU-1; N > M ; N--) // Test each element from the end to the beginning
            {
			if (scores[N] > scores[N-1] ) // Compare each element and swap if necessary
            {
			 temp = scores[N]; //place the index of the score in a temporary variable
			 temp2 = IDs[N]; //place the index of the ID in a temporary variable
             scores[N] = scores[N-1]; // decrement the value of N for test scores
			 IDs[N] = IDs[N-1]; // decrement the value of N for the ID number
             scores[N-1] = temp; //place the new index of scores into the temporary variable
			 IDs[N-1] = temp2; //place the new index of ID number into the temporary variable
			}
            }
      }
       return;  
 	}     

	void ExchangeSort(int id[], float test[], int &EU)
	{
            // Recieves - The array of student IDs, array of test scores, and elements used
            // Task - Sorts the array from lowest ID number to highest ID number
            // Returns - The sorted arrays.
	int M,N, Max;
	float temp, temp2;
     for ( M = 0 ; M < EU-1; M++) // Loop through every element in the array
     {
      Max = M;
		  for ( N =EU-1 ; N>M ; N--) // Test each element from the end to the beginning
			 {
			 if (id[N] > id[Max] ) // Compare each element and swap if necessary
             {
				temp = id[N]; //place the index of the ID in a temporary variable
			 }
				temp2 = test[N]; //place the index of the score in a temporary variable
				id[N] = id[Max]; //Place the max value index into the running ID index
				test[N] = test [Max]; // Place the max value index into the running test score index
				id[Max] = temp; //Place the max index into the ID temporary variable
				test[Max] = temp2; //Place the max index into the test score index
			 
			 }		
	 }
	 return;
	}          

	void FindLowScore(float scores[], int &elementsUsed, int &ls)
			// Recieves - the array of test scores, elements used, and the low score
			// Task - Finds the lowest score location in the array of test scores
			// Returns - The lowest score location in the array
	{
	int i;
	int minVal = 110; //Initialize a minimum value that is greater than the maximum value, which is 100
	ls = 0;
	for(i=0; i<elementsUsed; i++) //Continue in the loop until each value read in has been checked
	{
		if(minVal > scores[i]) //Check to see if the minimum value is greater than the current array value 
		{
			minVal = scores[i]; //Replace the minimum value with the current array value
			ls = i; //Replace the index location of the minimum with the value assigned to ls 
		}
	}
	return;
	}

	void FindHighScore(float scores[], int &elementsUsed, int &hs)
			// Recieves - The array of student IDs, elements used, and the high score
			// Task - Finds the highest score location in the array of test scores
			// Returns - The highest score location in the array
	{
	int i;
	int maxVal = -1; //Initialize a maximum value that less than the minimum value, which is 0
	for(i=0; i<elementsUsed; i++) //Continue in the loop until each value read in has been check
	{
		if(maxVal < scores[i]) //Check to see if the maximum value is less than the current array value
		{
			maxVal = scores[i]; //Replace the maximum value with the current array value
			hs = i; //Replace the index location of the maximum with the value assigned to hs 
		}
	}
	return;
	}

	void CalculateSum(float scores[], int &elementsUsed, float &total)
			// Recieves - the array of scores, elements used, and the total
			// Task - Compute the total sum of all the test scores
			// Returns - The sum of all the test scores
	{
	int i;
	total = 0;

	for(i=0; i<elementsUsed; ++i) //Check each value read in
	{
		total += scores[i]; //Add the test score in the current array value to total
	}
	total=total/elementsUsed;
	return;
	}

	void PrintScores(ofstream &outfile, float scores[], int id[], int &low, int &high, float &sum, int &EU)
			// Recieves - the array of scores, the min, the max, the sum, and the elements used
			// Task - Prints out the min score, max score, the average, and the elements used
			// Returns - The sum of all the test scores
	{
	//print the lowest test score and corresponding ID number
	outfile << "The lowest test score was " << scores[low] <<" achieved by student #"<< id[low] << endl;
	outfile << endl;
	// print the highest test score and corresponding ID number
	outfile << "The highest test score was " << scores[high] <<" achieved by student #"<< id[high] << endl;
	outfile << endl;
	// print the average test score of all the scores in the array 
	outfile << "The average test score for the group is " << sum << endl;
	return;
	}
