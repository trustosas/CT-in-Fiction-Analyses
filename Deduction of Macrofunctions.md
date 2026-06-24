### 1. Fundamental Definitions

Let  $\Phi$  be the set of **Macrofunctions**:

$$
\Phi =\{T,F,S,N\}
$$

We define the semantic properties of these functions using a feature space  $F$  consisting of two binary attributes derived from the text: **Format** ( $L$ ) and **Context** ( $C$ ).

#### 1.1 Attribute Definitions

1.  **Format (Literalness):**
    
    *    $L$  (Literal): Information expressed as explicit, observable, or axiomatic objects.
        
    *    $\neg L$  (Non-literal): Information expressed as associative, allusory, or essential qualities.
        
2.  **Context (Situationality):**
    
    *    $C$  (Situational): Information dependent on context, subjects, or animate movement.
        
    *    $\neg C$  (Universal/Removed): Information abstracted from context, generalized, or acontextual.
        

#### 1.2 Function Axioms

Based on the text's definitions ("S is situational and T is removed," "N is generalizing," "S+T are both literal," "N+F are both non-literal"), we define the mapping  $M:\Phi →\{L,\neg L\}\times \{C,\neg C\}$ :

*   **Sensing (S):**  $\{L,C\}$  (Literal, Situational)
    
*   **Thinking (T):**  $\{L,\neg C\}$  (Literal, Universal)
    
*   **Feeling (F):**  $\{\neg L,C\}$  (Non-literal, Situational)
    
*   **Intuition (N):**  $\{\neg L,\neg C\}$  (Non-literal, Universal)
    

Note that this forms a complete combinatoric space (a Punnett square of cognition).