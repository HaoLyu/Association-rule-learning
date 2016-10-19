# Author: Hao Lyu 
# UT Austin, School of Information
# Run by: python Apriori.py input_file.csv result_file sigma row_size
# For example: python Apriori.py input_file_example.dat result.csv 4 1000
# Explain: sigma is the support score, row_size is how many rows in the file you want to run(default None will run all the rows).
# Input format: Show as input_file_example.dat
# Output format: set_size,frequency,items
import sys
import time
import csv
from itertools import combinations

# Join n-item set itself and generate (n+1)-item set
# Then prune all the set that are not frequent
def join_prune(k):
	rt = {}
	if not any(k):
		return 

	keys = k.keys()
	if len(keys) < 2:
		return 

	leng = len(keys[0])
	candidates = []

	for i in range(len(keys)):
		for j in range(i+1, len(keys)):
			union = set(keys[i] + keys[j])
			if len(union) > leng+1:
				continue
			new_key = tuple(sorted(union))
			if new_key not in rt:
				prute_yes_no = True
				for m in range(len(new_key)):
					sub_key = new_key[:m]+ new_key[m+1:]
					if sub_key not in k:
						prute_yes_no = False
						break
				if prute_yes_no:
					rt[new_key] = 0
	return rt	

# Join n-item set itself and generate (n+1)-item set
def join_set(k):
	rt = {}
	if not any(k):
		return 

	keys = k.keys()
	if len(keys) < 2:
		return 

	leng = len(keys[0])
	candidates = []

	for i in range(len(keys)):
		for j in range(i+1, len(keys)):
			union = set(keys[i] + keys[j])
			if len(union) > leng+1:
				continue
			new_key = tuple(sorted(union))
			if new_key not in rt:
				rt[new_key] = 0
	return rt			

# Main function
def solution(data_file, result_file, sigma, row_size=0):
	# Remove lines containing fewer than 3 items
	matrix = []

	if row_size!= 0:
		stop = 0
		for row in open(data_file):
			if stop == row_size:
				break
			item = row.strip().split(' ')
			if len(item)>=3:
				item =[int(one_item) for one_item in item]
				matrix.append(item)
			stop += 1
	else:
		for row in open(data_file):
			item = row.strip().split(' ')
			if len(item)>=3:
				item =[int(one_item) for one_item in item]
				matrix.append(item)

	avg_row_leng = 1 + sum([len(row) for row in matrix])/(len(matrix))

	with open(result_file, 'w') as csvfile:
		fieldnames = ['set_size', 'frequency', 'items']
		writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
		writer.writeheader()

	# 1st scan and generate 1-item set
	L1,C1 = {},{}
	start_time = time.time()
	for transaction in matrix:
		for item in transaction:
			item_key = (item,)
			if item_key not in C1:
				C1[item_key] = 1
			else:
				C1[item_key] += 1

	for item in C1:
		if C1[item]>=sigma:
			L1[item] = C1[item]
	print "round 1"
	print "1-item set length:", len(L1.keys())
	Lk = L1
	Ck = {}
	del L1, C1
	k = 1
	skip_trans = {}

	# Loop and generate all frequent itemsets
	while any(Lk):
		k += 1
		print "round %d "%k
		if k<=2:
			Ck = join_set(Lk) 
		else:
			Ck = join_prune(Lk)
		Lk = {}
		if Ck is None:
			break

		# Optimize the speed and choose the faster way to generate candidate 
		len_candidate = len(Ck.keys())
		possible_group = 1
		for counter in range(k):
			possible_group *= avg_row_leng
			avg_row_leng -= 1

		if possible_group > 2* len_candidate:
			for transaction_idx in range(len(matrix)):
				if transaction_idx in skip_trans:
					continue
				transaction = set(matrix[transaction_idx])
				for one_candidate in Ck:
					set_one_candtdate = set(one_candidate)
					if set_one_candtdate.issubset(transaction):
						Ck[one_candidate] += 1
			for candidate in Ck:
				if Ck[candidate] >= sigma:
					Lk[candidate] = Ck[candidate]
			del Ck
		
		else:
			for transaction_idx in range(len(matrix)):
				if transaction_idx in skip_trans:
					continue
				transaction = matrix[transaction_idx]
				for one_candidate in combinations(transaction, k):
					if one_candidate in Ck:
						Ck[one_candidate] += 1				
			for candidate in Ck:
				if Ck[candidate] >= sigma:
					Lk[candidate] = Ck[candidate]
			del Ck

		# Generate skip rows in matrix 
		for transaction_idx in range(len(matrix)):
			if transaction_idx in skip_trans:
				continue
			trans_exist = True
			transaction = set(matrix[transaction_idx])
			for one_candidate in Lk:
				set_one_candtdate = set(one_candidate)
				if set_one_candtdate.issubset(transaction):
					trans_exist = False
					break
			if trans_exist:
				skip_trans[transaction_idx] = 1
				
		print "%d-item frequent set length:%d "%(k,len(Lk))
		if k>2:
			with open(result_file, 'a') as csvfile:
				fieldnames = ['set_size', 'frequency', 'items']
				writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
				for key in Lk:
					writer.writerow({'set_size': k,
									 'frequency': Lk[key], 
									 'items': ' '.join([str(item) for item in key])
									})

	print("--- %s seconds ---" % (time.time() - start_time))

if __name__ == '__main__':
	result_file = 'result.csv'
	sigma = 4		
	row_size = 0
	data_file = 'input_file_example.dat'

	if len(sys.argv) >2 :
		try:
			data_file = sys.argv[1]
			result_file = sys.argv[2]
			sigma = int(sys.argv[3])
			row_size = int(sys.argv[4])
		except IndexError:
			pass
	solution(data_file, result_file, sigma, row_size)
