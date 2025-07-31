else:
                    # 在符合条件的难度中随机选择
                    valid_diff = difficulties[idx:]
                    if not valid_diff:
                        selected_diff = difficulties[-1]
                    else:
                        # 更倾向于选择接近目标难度中值的题目
                        target_diff = sum(difficulty_range) / 2
                        closest_diff = min(valid_diff, key=lambda x: abs(x - target_diff))
                        selected_diff = closest_diff
                
                # 从选中的难度级别中随机取题
                questions = type_pool[selected_diff]
                if questions:
                    take_num = min(remaining, len(questions))
                    selected extend(random sample(questions, take_num))
                    remaining -= take_num
                    total_difficulty += selected_diff * take_num
                
                # 如果这个难度级别的题目用完了，移除它
                if selected_diff in type_pool and not type_pool[selected_diff]:
                    del type_pool[selected_diff]
                    difficulties remove(selected_diff)
            
            exam_questions extend(selected)
        
        # 计算实际平均难度
        if exam_questions:
            actual_diff = total_difficulty / len(exam_questions)
        else:
            actual_diff = sum(difficulty_range) / 2
        
        # 打乱题目顺序
        random shuffle(exam_questions)
        
        return {
            'questions': exam_questions,
            'actual_difficulty': round(actual_diff, 2),
            'question_count': len(exam_questions)
        }
