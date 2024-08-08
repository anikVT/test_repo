# test_repo

import numpy as np
from scipy.linalg import svd

def quat2rotmat(quat):
    # This function should convert a quaternion to a rotation matrix
    pass

def rotmat2quat(rotmat):
    # This function should convert a rotation matrix to a quaternion
    pass

def align_quaternions(setA, setB):
    n = setA.shape[0]
    
    # Convert quaternions to rotation matrices
    rotA = np.array([quat2rotmat(q) for q in setA])
    rotB = np.array([quat2rotmat(q) for q in setB])
    
    # Compute the matrix K
    K = np.zeros((3, 3))
    for i in range(n):
        K += np.dot(rotB[i].T, rotA[i])
    
    # Perform SVD on K
    U, S, Vt = svd(K)
    R_opt = np.dot(U, Vt)
    
    # Ensure a proper rotation (det(R_opt) = 1)
    if np.linalg.det(R_opt) < 0:
        Vt[2, :] *= -1
        R_opt = np.dot(U, Vt)
    
    # Convert the optimal rotation matrix to a quaternion
    q_opt = rotmat2quat(R_opt)
    
    return q_opt

# Example usage with your sets of quaternions
setA = np.array([...])  # Replace with your set A quaternions (n,4)
setB = np.array([...])  # Replace with your set B quaternions (n,4)

optimal_quat = align_quaternions(setA, setB)
print("Optimal Quaternion to align Set B to Set A:", optimal_quat)
